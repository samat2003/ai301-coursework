# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives:** In an eval bundle, compare the issue body and thread highlights with the repro report's environment line; repo facts may identify the expected version or platform. In live mode, read the issue and relevant comments on GitHub, then the student's draft repro comment. The draft must carry the environment evidence readers need.

**What good looks like:** The attempt identifies the tool version, OS or platform, and any runtime, build, driver, shell, dependency, or configuration that the issue links to its trigger. Compare these fields with the issue's target. If a material field differs, the report names the difference and limits what the attempt establishes. A different OS does not automatically matter to a platform-independent issue; a changed shell does matter when the failure is shell-specific.

## Steps

**Where it lives:** In an eval bundle, read the issue's trigger, then the repro report's starting state, input, config, commands, and actions. In live mode, compare the GitHub issue with the student's draft repro comment and anything pasted or linked as part of that draft. Unposted local files are not evidence a reader will have.

**What good looks like:** A stranger can reconstruct the relevant files and settings and run the trigger without guessing. A short command can be enough; complex cases need the actual minimal input or configuration, either in the report or precisely identified in the issue included with it. If the test depends on a private project or redacted config, supply a shareable minimal case before calling the steps followable. The steps should exercise the issue's trigger rather than a superficially similar command.

## Behavior shown

**Where it lives:** In an eval bundle, compare the issue body's current and expected behavior and thread clarifications with the repro report's output, log excerpt, screenshot description, measurements, or observed result. In live mode, compare the issue thread on GitHub with the artifact included in the draft repro comment.

**What good looks like:** The artifact demonstrates the specific reported symptom: the same error or crash mode, missing output, UI state, or other distinguishing behavior. Compare exit status, error text, and trigger when they separate the reported problem from an adjacent one. A cannot-reproduce report instead shows the attempted trigger and the nonfailing result. A version banner, ordinary operation, or a claim that something failed is not itself proof of the failure.

## Honesty

**Where it lives:** In an eval bundle, put the claim comment and the repro report's conclusion beside the shown artifacts and environment; check statements about number of runs, versions, platforms, certainty, and root cause. In live mode, do the same with both draft comments against their pasted evidence and the issue thread.

**What good looks like:** The words state what was actually tried and seen, including a failed attempt to reproduce. A report may suggest a cause or a setup difference as a hypothesis, clearly labeled. For repeated attempts, representative output plus a specific account of how many other runs were made and what they showed can support a bounded repeatability claim; merely saying "always" cannot. Claims about another platform or a verified cause need evidence for that broader scope.

## Comms

**Where it lives:** In an eval bundle, compare the candidate claim comment with the issue's concrete symptom, and read repo facts for bug-report expectations and contribution or AI-use policy. Check both candidate comments for any required disclosure. In live mode, read the issue thread and the repository's CONTRIBUTING guide, issue template, and stated AI policy on GitHub, then compare the student's claim and repro drafts.

**What good looks like:** The claim names a recognizable error or trigger and says what the writer will investigate or report next without promising a fix or deadline. The repro comment presents evidence in a form other contributors can use. For eval, apply disclosure rules as though candidate comments received AI assistance; for live drafts, check the student's actual use. When disclosure is required, look for the tool and extent of help if the policy asks for both. Do not invent a disclosure requirement where the repo is silent or permissive. Specific, modest wording matters more than matching headings or sounding polished.
