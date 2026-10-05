# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis is grounded | Read the candidate plan's stated cause or diagnosis against the package's Repro evidence, especially the observed actual behavior, controls, and any evidence that isolates the failure. | Pass only if the proposed cause explains the behavior the reproduction actually demonstrates and does not contradict or ignore stronger repro evidence. A cause copied only from issue/thread speculation does not pass when the reproduction points somewhere else. The plan must address the cause supported by the evidence rather than only a symptom. | required |
| Scope is bounded | Read the candidate plan's scope, named files or areas, and proposed changes against the Issue and Repro evidence. | Pass if the work is one bounded change tied to the supported diagnosis, with the files or areas to change identified closely enough to constrain the implementation. Any additional work must be necessary for the fix or its regression coverage. Unrelated cleanup, rewrites, or unexplained expansion fails. | required |
| Approach is executable | Read the candidate plan's files or areas and implementation approach, using Repo facts and Issue context when needed to understand the named targets. | Pass if another contributor could begin the implementation without having to ask what core behavior to change or where to start. The plan must identify the relevant code area and describe the intended behavior change concretely; exact line numbers or exhaustive implementation details are not required. | required |
| Test proves the fix | Read the candidate plan's Test or test-plan text against the Repro evidence's steps, expected result, actual result, and relevant controls. | Pass if the test would observably distinguish the fixed behavior from the reproduced bug. It should re-run the original repro or a faithful equivalent through the real behavior being changed and state what should be different after the fix. Generic statements such as "run tests" or "make sure it works" are insufficient by themselves. | required |
| Claims are honest about uncertainty | Read the candidate plan's diagnosis, approach, risks, unknowns, and other certainty claims against the Repro evidence and Issue context. | Pass if factual claims used to justify the implementation are supported by the available evidence, and any unresolved question that could materially change the approach or test is identified as an unknown rather than stated as fact. A plan does not need a separate risks section when no material uncertainty is present. | required |
| Comment fits thread and repo conventions | Read the Candidate plan comment against Thread highlights and the Repo facts block, especially maintainer instructions, contribution policy, templates, and any stated AI-use or disclosure requirement. | Pass if the comment does not ignore or contradict relevant maintainer direction or repository contribution requirements. It must include any disclosure or coordination requirement that the repo explicitly makes relevant. Do not fail it for omitting repo facts or template fields that are unrelated to posting the plan. | required |

## Verdict rule

Return `accept` only when every required check is `pass`.

If any required check is `fail`, return `reject`.

Treat `unclear` on a required check as a failure and return `reject`, because evidence that cannot be verified from the package is not ready to build from.

Preferred checks, if any are added later, never change the verdict; a preferred `fail` or `unclear` may be reported but does not cause rejection.
