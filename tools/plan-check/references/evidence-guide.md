# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives**

- In eval mode, read the Candidate plan's diagnosis, cause, summary, or approach statements against the package's `Repro evidence` section. Use the Issue and Thread highlights only as context; do not treat an unsupported thread claim as stronger than reproduced observations.
- In live mode, read the diagnosis in `plan.md` and `comment.md` against the student's posted Unit 2 reproduction comment on the scoped GitHub issue. For a house issue, use the house repro evidence quoted in the drafts as described by `SKILL.md`.
- In the repro evidence, look especially at the failing steps, expected versus actual behavior, controls, comparisons, traces, timings, or other observations that isolate when the bug does and does not occur.

**What good looks like**

The plan's stated cause explains the behavior that was actually reproduced and does not conflict with stronger observations or controls. A diagnosis should follow from the evidence rather than merely repeat a theory from the issue thread. If the reproduction isolates the problem away from the proposed cause, the diagnosis is not grounded.

## Scope

**Where it lives**

- In eval mode, read the Candidate plan's scope, change list, exclusions, named files, and implementation targets against the Issue and Repro evidence.
- In live mode, read the scope, files-to-touch, approach, and explicit out-of-scope statements in `plan.md` and `comment.md`.
- Use the issue and reproduction to determine which proposed work is necessary for the supported fix and its regression coverage.

**What good looks like**

The plan describes one bounded change tied to the supported diagnosis. It identifies the relevant file, files, component, or code area closely enough to constrain implementation, and any extra work is necessary to implement or verify the fix. Unrelated cleanup, broad rewrites, opportunistic refactors, or unexplained expansion are outside a good scope.

## Executability

**Where it lives**

- In eval mode, read the Candidate plan's named files or code areas, approach, ordered change description, and intended behavior.
- In live mode, read the implementation approach and files-to-touch in `plan.md` and the concise change description in `comment.md`.
- Repo facts may be used only when they provide relevant conventions or context for the named implementation target; do not use them to invent missing implementation steps.

**What good looks like**

A contributor unfamiliar with the author's private reasoning can identify where to start and what core behavior must change. Exact line numbers, pseudocode, or exhaustive implementation details are not required, but the plan cannot stop at vague instructions such as "fix the bug", "update the logic", or "change the relevant code" with no concrete starting area or behavior.

## Test plan

**Where it lives**

- In eval mode, read the Candidate plan's test or validation section against the package's `Repro evidence` steps, expected result, actual result, and controls.
- In live mode, read the test plan in `plan.md` and compare it with the commands, steps, and observed failure from the student's Unit 2 reproduction.
- Look for the trigger that originally exposed the bug, the observable failing result, and the observable result expected after the fix. Also note any useful non-failing control that should continue to work.

**What good looks like**

The proposed validation would distinguish the fixed behavior from the reproduced failure. Re-running the original reproduction is ideal when possible; a faithful equivalent is acceptable when it exercises the real changed code and checks the same behavior. A generic statement like "run tests", "verify it works", or "run CI" is not sufficient by itself unless it is paired with a concrete observable that proves this bug is gone.

## Honesty

**Where it lives**

- In eval mode, read factual and certainty claims throughout the Candidate plan, especially the diagnosis, approach, risks, unknowns, and any claimed side effects, against the Issue and Repro evidence.
- In live mode, read `plan.md`'s diagnosis, risks/unknowns, and `## Deviations` section when present. Also compare strong claims in `comment.md` against the available reproduction evidence.
- During a post-build re-check, a difference between the original plan and what was actually built belongs under `## Deviations` in `plan.md`.

**What good looks like**

Claims that drive the implementation are supported by available evidence, and a material uncertainty is labeled as an uncertainty instead of being presented as proven. The plan does not need to invent risks merely to fill a section. After implementation, a real deviation is recorded with what changed and why; if there was no deviation, the section should say so in the student's own words rather than remain blank.

## Comms

**Where it lives**

- In eval mode, read the Candidate plan comment against `Thread highlights` and `Repo facts`, especially explicit maintainer directions, contribution policy, coordination instructions, templates, and any AI-use or disclosure requirement.
- In live mode, read the draft `comment.md` against the live GitHub issue thread and the repository's stated contribution instructions allowed by `SKILL.md`. Also apply `voice-guide.md` separately as required by `SKILL.md`.
- Only use requirements actually stated in those sources. Do not treat general preferences or unstated best practices as repository rules.

**What good looks like**

The plan comment respects relevant maintainer direction and repository contribution requirements and does not contradict important thread context. When an explicit disclosure, coordination, or posting requirement applies, the comment satisfies it. The comment does not need to repeat unrelated issue-template fields or repo facts that do not affect posting the plan.
