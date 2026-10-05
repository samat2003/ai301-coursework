# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

samat2003

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68#issuecomment-5986903482

I reproduced this on commit `f89c06f` (Python 3.12.10, `rank-bm25` 0.2.2) and I'd like to take it. Here is my plan.

**Cause.** `KeywordSearcher.index([])` builds an empty `tokenized_corpus` and passes it to `BM25Okapi` (`rag/retriever/keyword_search.py`, line 25). My traceback ends in `rank_bm25.py`, at `self.avgdl = num_doc / self.corpus_size`, with `ZeroDivisionError: division by zero`. This isn't a `search()` problem: `KeywordSearcher().search('python', top_k=10)` on a never-indexed searcher already logs `keyword_search_empty_index` and returns `[]`, and indexing a one-chunk corpus builds fine and returns a scored result. `index()` just never lets an empty corpus reach that guard.

**Change.** In `KeywordSearcher.index()`, keep storing the supplied chunks. If the list is empty, set `self.bm25 = None` and return before tokenizing or constructing `BM25Okapi`. That leaves the searcher in the same state as a never-indexed one, so the existing guard in `search()` returns `[]`. Non-empty indexing is unchanged.

**Files.**
- `rag/retriever/keyword_search.py`
- `tests/unit/test_keyword_search.py`: remove the strict `xfail` marker from `test_empty_index`, and add one regression test that indexes a populated corpus, then calls `index([])`, then checks that `search()` returns `[]` and nothing comes from the old index.

**Validation.**
- Re-run my original repro, `KeywordSearcher().index([])`. It currently raises `ZeroDivisionError`; after the fix it should exit normally.
- `index([])` followed by `search("python")` should return `[]`.
- `tests/unit/test_keyword_search.py` currently gives `16 passed, 1 xfailed`. After the fix, `test_empty_index` should be a normal pass with no `XFAIL`.
- The non-empty control should still build the index and return a scored result.
- Then `make test-unit`, `make lint` and `make typecheck`.

**Out of scope.** Changes to `rank_bm25`, tokenization, BM25 scoring, other retriever behavior, and unrelated cleanup.

One open question: I haven't added a log line for the empty case in `index()`, since nothing in the issue asks for one. Let me know if you'd like one. I'll work on a branch in my fork and open a PR that references this issue.

---

## Your branch

**Branch**

fix/68-keyword-search-empty-index

**Evidence**

I re-ran my Unit 2 reproduction against the built change (commit `ca753d8`). BEFORE is my Unit 2 run on `f89c06f`; AFTER is the same commands on the fix branch.

### BEFORE: Unit 2 reproduction on `f89c06f`

```
$ ./.venv-repro/Scripts/python.exe -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([])"

Traceback (most recent call last):
  ...
  File "rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
  ...
  File "rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero

$ ./.venv-repro/Scripts/python.exe -m pytest tests/unit/test_keyword_search.py -q -rx

........x........ [100%]

XFAIL tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index - issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index
16 passed, 1 xfailed in 0.16s
```

### AFTER: `fix/68-keyword-search-empty-index`

Original empty-index reproduction:

```
$ ./.venv-repro/Scripts/python.exe -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print('index_empty_ok')"

index_empty_ok
```

Empty index followed by search:

```
$ ./.venv-repro/Scripts/python.exe -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([]); print(s.search('python', top_k=10))"

2026-10-04 22:12:46 [warning  ] keyword_search_empty_index
[]
```

Focused test file (`test_empty_index` is now a normal pass, and the new populated-to-empty regression test is included):

```
$ ./.venv-repro/Scripts/python.exe -m pytest tests/unit/test_keyword_search.py -q -rx

..................                                                       [100%]
18 passed in 0.14s
```

Non-empty control:

```
$ ./.venv-repro/Scripts/python.exe -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([{'id': 1, 'text': 'python content'}]); print(s.search('python', top_k=10))"

[info] keyword_index_built            chunk_count=1
[info] keyword_search_complete        query_len=1 results_count=1
[{'id': 1, 'text': 'python content', 'bm25_score': -0.2746530721670274}]
```

Populated-to-empty result:

```
before_clear= [{'id': 1, 'text': 'python content', 'bm25_score': -0.2746530721670274}]
[warning  ] keyword_search_empty_index
after_clear= []
```

Broader repository validation:

`make test-unit`, `make lint`, and `make typecheck` could not be run in this local environment because `make` is unavailable and the checkout does not contain the Makefile's expected `.venv/Scripts` tooling.

A direct substitute unit-suite attempt produced:

```
293 passed, 32 xfailed, 6 collection errors
```

The six collection errors were caused by missing unrelated dependencies in `.venv-repro` (`jose`, `pypdf`, `redis`, `sqlalchemy`, and `tiktoken`). I did not treat that run as a passing full suite. The focused Issue #68 test file passed 18/18.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

- 20/20 — PASS

This was the only completed scored full run. An earlier launch failed before grading because of a Windows text-encoding error and produced no agreement score or `eval-run.txt`.

**Package analysis**

Package: `pkg-01` (category `wrong-cause`). My rubric's verdict was `reject`, and the gold label was `reject`.

The candidate plan says the root cause is HTTPie's request-item tokenizer in `httpie/cli/requestitems.py`. But the package's repro evidence says `--debug` shows the failure occurs inside argparse's `parse_args` while consuming positional arguments, and that "the request items are never handed to HTTPie's item parser." So the plan's diagnosis contradicts the strongest reproduction evidence and targets a component the reproduction says is never reached. That makes the required `Diagnosis is grounded` check fail, and any required fail means `reject`. I did not run a separate retry for `pkg-01`.

**Check rationale**

The check I'm quoting is `Diagnosis is grounded`, copied from `tools/plan-check/rubric.md` as it reads now:

```
| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis is grounded | Read the candidate plan's stated cause or diagnosis against the package's Repro evidence, especially the observed actual behavior, controls, and any evidence that isolates the failure. | Pass only if the proposed cause explains the behavior the reproduction actually demonstrates and does not contradict or ignore stronger repro evidence. A cause copied only from issue/thread speculation does not pass when the reproduction points somewhere else. The plan must address the cause supported by the evidence rather than only a symptom. | required |
```

I wrote it this way because of `calib-03` in the classroom calibration exercise. Its plan blamed pager key bindings, but its reproduction showed the slowdown remained even with no pager at all and tracked the syntax-highlighting work. A plausible diagnosis can still be wrong when it follows issue-thread speculation instead of the reproduction. So I rejected a weaker rule that only asked whether the diagnosis sounded plausible or matched the thread. The final check compares the proposed cause against the observed repro behavior and the stronger controls.

**Trade-offs**

The stricter evidence-grounding requirement deliberately favors false negatives over approving an unsupported cause. If a reproduction is too incomplete to establish whether the proposed cause follows from it, this required check can come out `unclear`, and my verdict rule treats `unclear` as a rejection even when the contributor's theory might turn out to be correct. That is the cost of requiring a plan to be build-ready from evidence rather than from confidence or thread speculation.

In my completed run this strictness did not flip any scored clear-accept package: the final eval agreed on all 20 packages, including `clear-accept 7/7`. That does not prove the rubric can never produce a false negative.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
