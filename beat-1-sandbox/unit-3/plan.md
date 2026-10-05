# Plan for Issue #68

## Diagnosis

`KeywordSearcher.index([])` builds an empty `tokenized_corpus` (a list comprehension over zero chunks) and passes it straight to `BM25Okapi(tokenized_corpus)` at `rag/retriever/keyword_search.py` line 25. `BM25Okapi` computes `self.avgdl = num_doc / self.corpus_size` during initialization, and with an empty corpus `corpus_size` is 0, so it raises `ZeroDivisionError`. The crash happens inside `index()`, before `search()` is ever reached.

`search()` is not the problem. It already guards with `if not self.bm25 or not self.chunks:` and returns `[]` (logging `keyword_search_empty_index`). My control that calls `search()` on a never-indexed searcher confirms that path works. So the missing piece is that `index()` never lets an empty corpus reach that guard.

## Repro evidence

From my Unit 2 reproduction (commit `f89c06f`, Python 3.12.10, `rank-bm25` 0.2.2):

- Failing command: `KeywordSearcher(); s.index([])`
- Traceback points at the BM25 construction in `index()`:
  - `File "rag/retriever/keyword_search.py", line 25, in index`
  - `self.bm25 = BM25Okapi(tokenized_corpus)`
  - `self.avgdl = num_doc / self.corpus_size`
  - `ZeroDivisionError: division by zero`
- Non-empty control (`index([{'id': 1, 'text': 'python content'}])` then `search('python', top_k=10)`): builds the index and returns one scored result, so BM25 itself works for non-empty input.
- Empty/uninitialized-search control (`KeywordSearcher().search('python', top_k=10)`): logs `keyword_search_empty_index` and returns `[]`, so the empty-index guard in `search()` already works.
- Focused test run: `16 passed, 1 xfailed in 0.16s`, with `XFAIL ... test_empty_index - issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index`.

## Scope

In scope:

1. Handle an empty `chunks` list inside `KeywordSearcher.index()` before `BM25Okapi` is constructed.
2. Preserve the supplied (empty) chunks in `self.chunks`.
3. Make sure no stale BM25 index remains when an empty corpus is indexed (`self.bm25` is `None` afterwards).
4. Remove the strict `xfail` marker for Issue #68 from `test_empty_index`.
5. Add one narrowly targeted regression test showing that re-indexing from a populated corpus to an empty one does not leave stale searchable data.

Out of scope:

- Changing `rank_bm25` or its behavior.
- Redesigning tokenization.
- Changing BM25 scoring.
- Broad retriever refactoring.
- Unrelated search behavior.
- Unrelated cleanup.

## Files to change

- `rag/retriever/keyword_search.py`
- `tests/unit/test_keyword_search.py`

## Approach

In `KeywordSearcher.index()`:

1. Keep storing the supplied chunks in `self.chunks`.
2. If the chunk list is empty, set `self.bm25 = None` and return before tokenization and `BM25Okapi` construction.
3. That leaves the searcher in the same state as a never-indexed one, so the existing empty-index guard in `search()` returns `[]` with no further change to `search()`.
4. Non-empty input continues through the existing tokenize-and-`BM25Okapi` path unchanged.

In `tests/unit/test_keyword_search.py`:

5. Remove the `@pytest.mark.xfail(strict=True, ...)` decorator from `test_empty_index` (it is strict, so it would report a failure once the bug is fixed if left in place).
6. Add a regression test: index a non-empty corpus, call `index([])`, then confirm `search("python")` returns `[]` and no result comes from the old corpus.

## Test plan

Success is: no `ZeroDivisionError`, `[]` from searching an emptied index, and `test_empty_index` reporting as a normal pass instead of `XFAIL`.

1. Re-run my original reproduction: `KeywordSearcher().index([])`. Before the fix: `ZeroDivisionError: division by zero`. After the fix: exits normally with no traceback.
2. Run `index([])` followed by `search("python")`. Expected: `[]`.
3. Re-run `tests/unit/test_keyword_search.py`. Before: `16 passed, 1 xfailed`. After: all tests pass, `test_empty_index` is a normal pass, and there is no `XFAIL` line (17 original tests passing plus the new regression test).
4. Re-run the non-empty control (`index([{'id': 1, 'text': 'python content'}])` then `search('python', top_k=10)`). Expected: still builds the index and returns one scored result.
5. Run the new populated-then-empty regression test: index data, then `index([])`, then `search(...)`. Expected: `[]` with nothing from the old index.
6. After the focused tests, run the repo's own checks as defined in the `Makefile` and `.github/workflows/ci.yml`: `make test-unit`, `make lint` (`ruff check .`), and `make typecheck` (`mypy api/ core/ ingestion/ rag/ agent/ safety/`). The PR template also asks for CI to be green.

## Risks and unknowns

- Stale state: if a searcher is indexed with data and later receives `index([])`, `self.bm25` would still point at the old model unless it is cleared. The existing `search()` guard also checks `not self.chunks`, so searching would likely still return `[]` even with a stale model, but I have not verified that and clearing `self.bm25` removes the dependency on it. The populated-then-empty test covers this case.
- Unknown: whether the maintainers want a log line for the empty-index case in `index()`. I will not add one unless asked, since nothing in the issue requires it.
- I reproduced this only on commit `f89c06f` with `rank-bm25` 0.2.2 on Windows (Python 3.12.10). CI runs Python 3.11, and I have not run the fix there.

## Deviations

The implementation matched the plan: the empty-corpus guard, BM25 state reset, xfail removal, and populated-to-empty regression test were implemented without changing the planned scope, and there were no source or test deviations. The only limitation was the validation environment: this checkout does not have the Makefile's expected `.venv` tooling and `make` is unavailable, so I could not run `make test-unit`, `make lint`, or `make typecheck` locally. That is a limit on how I could validate, not a change to the plan. The focused Issue #68 reproduction, the keyword-search test file (18 passed), the non-empty control, and the populated-to-empty regression all ran and passed. I did not run the full repo suite to a clean pass; a substitute unit-suite run gave 293 passed, 32 xfailed, and 6 collection errors from unrelated missing dependencies.
