# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68

**Verdict output**

```
Ranked Candidates:
1. https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68
   Fit reason: Tier-1 starter bug in Python RAG retriever with clear repro steps and specific test files named.

Checks:
- repo-active: pass (Repo codepath/pathreview-ai301-fa26-s1 is active with recent commits.)
- unclaimed: pass (No assignee set and no open linked PRs on this issue.)
- bounded-scope: pass (Bounded bug fix handling empty corpus in KeywordSearcher.index().)
- ai-policy-allowed: pass (No restrictive AI contribution policy stated.)
- maintainer-liveness: pass (Active maintainer commits present in repository.)

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {
        "name": "repo-active",
        "grade": "pass",
        "evidence": "Repo codepath/pathreview-ai301-fa26-s1 is active with recent commits."
      },
      {
        "name": "unclaimed",
        "grade": "pass",
        "evidence": "No assignee set and no open linked PRs on this issue."
      },
      {
        "name": "bounded-scope",
        "grade": "pass",
        "evidence": "Bounded bug fix handling empty corpus in KeywordSearcher.index()."
      },
      {
        "name": "ai-policy-allowed",
        "grade": "pass",
        "evidence": "No restrictive AI contribution policy stated."
      },
      {
        "name": "maintainer-liveness",
        "grade": "pass",
        "evidence": "Active maintainer commits present in repository."
      }
    ],
    "verdict": "accept"
  }
]
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- Run 1: 20/20

**Issue analysis**

For `issue-12` (source: `bookwyrm-social/bookwyrm#1133`), my rubric decided `reject` and the gold label was `reject`.

Gold label reasoning: "passes every liveness, scope, and claim check; the repo's contributing docs ban AI-generated code and documentation outright"

My rubric evaluated this via the `ai-policy-allowed` check. In `issue-12.md`, the repo facts stated:
"contribution policy (CONTRIBUTING.md, section "Generative AI"): outright ban on AI-generated code and documentation"

Because my check `ai-policy-allowed` requires that "The project's contribution policy does NOT explicitly ban or prohibit AI-generated or AI-assisted contributions", `issue-12` failed `ai-policy-allowed` with evidence "Contributing policy explicitly bans AI-generated code and docs outright.", resulting in a verdict of `reject`.

**Check rationale**

 Quoting check `ai-policy-allowed` from `rubric.md`:

| `ai-policy-allowed` | Repo facts: `contribution policy` line (and referenced CONTRIBUTING/AI docs). | The project's contribution policy does NOT explicitly ban or prohibit AI-generated or AI-assisted contributions (silence or conditional requirements like disclosure/testing pass). | required |

Reasoning:
In this course, contributions are created using AI assistance. If a project's explicit policy prohibits AI-generated or AI-assisted code/documentation, submitting a pull request violates the project's rules and maintainer guidelines. Distinguishing between outright bans and conditional policies (such as required disclosure or testing) ensures that we respect maintainer boundaries without rejecting repositories that permit responsible AI usage.

**Trade-offs**

The `ai-policy-allowed` check strictly fails repos with an explicit ban on AI-generated content, while allowing repos that are silent or specify conditional requirements (e.g. disclosure, human verification, or testing). For instance, in `issue-08` (source: `zulip/zulip#39794`), the policy states: "AI tools allowed; contributors must personally understand, test, and be able to explain every change". The check correctly passes this policy because conditional requirements are allowed, whereas `issue-12` (source: `bookwyrm-social/bookwyrm#1133`) is rejected due to its outright ban.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. The issue is a Tier-1 starter bug in Python (`rag/retriever/keyword_search.py`), asking to fix a `ZeroDivisionError` when an empty corpus is passed to `KeywordSearcher.index()`, and to remove an `@pytest.mark.xfail` marker in `tests/unit/test_keyword_search.py`. With an estimated effort of 2–4 hours and well-defined Python test files, it perfectly fits my Python background and available time.
2. The verdict correctly identified that the repository is active, the issue is unclaimed with no assignees or open PRs, and the scope is cleanly bounded to one function and unit test. In addition to what the rubric checked, I weighed that the issue provides exact file paths and specific error types (`ZeroDivisionError`), which guarantees a straightforward setup and reproduction experience.
3. The anticipated difficulty in claiming it is low. Under Path Review classroom house rules, claim comments from classmates do not block claiming an issue, and the issue currently has no official assignee or linked PR.
