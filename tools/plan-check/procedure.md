# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Determine whether this is eval mode or live mode from the input described by `SKILL.md`. In eval mode, use only the package bundle. In live mode, use the live issue and the student's drafts as allowed by `SKILL.md`.
2. Before grading any rubric check, read the Issue context first. Record the reported bug, expected behavior, actual behavior, and any explicit limits or maintainer requests relevant to a fix.
3. Read the Repro evidence next, before reading the candidate plan. Record the reproduced steps, the observed failing behavior, expected result, controls or comparisons, and any evidence that narrows where the failure occurs. Treat these observations as the baseline the diagnosis must explain.
4. Read the Repo facts and Thread highlights next. Record only contribution rules, maintainer directions, conventions, or coordination requirements that could affect the plan or plan comment.
5. Read the Candidate plan after the issue and repro evidence. Record its stated diagnosis or cause, scope and exclusions, files or code areas, implementation approach, test plan, and any risks or unknowns it states.
6. Read the Candidate plan comment last. Record its description of the cause, intended change, test or validation claims, and any statements relevant to the thread or repository's contribution rules.
7. Do not decide whether a check passes while performing the initial read. Finish the evidence notes first so later plan claims are compared against the reproduction instead of being used to reinterpret it.

## Evidence gathering

1. For `Diagnosis is grounded`, place side by side:
   - the plan's stated cause or diagnosis, and
   - the Repro evidence observations and controls that support, contradict, or narrow that cause.
   Record the specific repro fact that most strongly decides whether the diagnosis follows from the evidence.
2. For `Scope is bounded`, gather:
   - the plan's in-scope change,
   - any explicit out-of-scope limits,
   - named files or code areas, and
   - the issue/repro behavior the change is meant to fix.
   Record any proposed work that does not directly support that fix or its regression coverage.
3. For `Approach is executable`, gather the plan's named implementation location and the concrete behavior it says will change. Record whether those details give another contributor a clear starting point without inventing missing core steps.
4. For `Test proves the fix`, gather the Repro evidence's trigger, observed failure, expected result, and relevant controls. Then gather the plan's proposed validation. Record exactly what observable result after the change would distinguish the fixed behavior from the reproduced failure.
5. For `Claims are honest about uncertainty`, compare factual or confident statements in the plan against the Issue and Repro evidence. Record any material assumption that is stated as fact without support, and record any material unresolved question that the plan explicitly identifies.
6. For `Comment fits thread and repo conventions`, gather only relevant directions from Thread highlights and Repo facts, including explicit contribution, coordination, template, or AI-disclosure requirements. Compare those directions with the Candidate plan comment and record any direct conflict or required item it omits.
7. If evidence for a check is genuinely absent from the allowed package or live sources, record that absence. Do not search unrelated files, infer hidden implementation facts, or substitute general software-engineering expectations for missing evidence.

## Check execution

1. Grade the checks in the same order they appear in `rubric.md`.
2. For each check, use only the evidence gathered for that check and apply its written pass condition literally. Do not tighten or loosen a check because the overall plan feels good or bad.
3. Give `pass` when the gathered evidence satisfies the pass condition.
4. Give `fail` when the gathered evidence shows that the pass condition is not satisfied or is contradicted.
5. Give `unclear` only when the allowed evidence is genuinely insufficient to determine whether the pass condition is satisfied. Do not use `unclear` merely because the plan is short; a short plan can pass when it provides enough evidence for the condition.
6. For every grade, write one concise evidence line naming the fact, comparison, omission, or contradiction that decided the result. Do not use statements such as `looks good` or `seems reasonable` as evidence.
7. For `Diagnosis is grounded`, give priority to observed Repro evidence over unsupported issue-thread speculation. If the plan adopts a claimed cause from the thread but the reproduction isolates the problem somewhere else, grade the diagnosis according to the reproduction.
8. For `Test proves the fix`, do not require a particular testing format or number of tests. Grade whether the proposed test would observably demonstrate that the reproduced bug is gone.
9. For `Approach is executable`, do not fail solely because exact line numbers, pseudocode, or exhaustive implementation details are absent. Fail only when the core change or starting location is too unspecified for another contributor to begin.
10. For `Comment fits thread and repo conventions`, do not invent repository requirements. Grade only requirements or maintainer instructions actually present in the allowed evidence.
11. Do not re-read the entire package between checks unless the evidence note for that check is incomplete. Return to the exact source section named by the rubric or evidence guide instead.

## Verdict assembly

1. After every rubric check has a grade and evidence line, apply the verdict rule from `rubric.md` exactly.
2. Treat `unclear` on a required check as a failure, as specified by the rubric.
3. Return `accept` only if every required check is `pass`.
4. Return `reject` if any required check is `fail` or `unclear`.
5. Preferred checks, if present, may be reported but must not change the verdict.
6. Before producing the result, verify that every rubric check appears exactly once in the output with a grade of `pass`, `fail`, or `unclear` and a one-line evidence statement.
7. In the readable summary, identify the check or checks responsible for a rejection and state the deciding evidence. In live mode, also include any voice-guide notes required by `SKILL.md`, but do not let a voice-guide note change the verdict unless a rubric check explicitly makes it verdict-relevant.
8. End with the valid fenced JSON block required by `SKILL.md`. Use `accept` or `reject` only, and put nothing after that JSON block.
