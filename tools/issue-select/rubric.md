# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-active` | Repo facts: `archived:` line, `last push to any branch` date, and `last 5 default-branch commits` dates. | Repo is not archived AND at least one push or default-branch commit occurred within 365 days of capture/current date. | required |
| `unclaimed` | Repo facts: `this issue: assignees:` and `linked PRs:`, plus the thread comments. | `assignees:` is none/empty AND `linked PRs:` has no open PRs AND there are no active, unabandoned claims or open PRs mentioned in thread comments. | required |
| `bounded-scope` | Issue title, body, checklist, and comment thread. | The issue describes a single bounded task, bug fix, or doc update. It is NOT a tracking/megaissue, umbrella task (e.g. codebase-wide type annotations), or open-ended design debate without a settled spec. | required |
| `ai-policy-allowed` | Repo facts: `contribution policy` line (and referenced CONTRIBUTING/AI docs). | The project's contribution policy does NOT explicitly ban or prohibit AI-generated or AI-assisted contributions (silence or conditional requirements like disclosure/testing pass). | required |
| `maintainer-liveness` | Repo facts: `last 5 default-branch commits` author list and `maintainer first-response sample`. | At least one human maintainer commit or maintainer response is present within 180 days of capture/current date. | preferred |

## Verdict rule

accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail.
