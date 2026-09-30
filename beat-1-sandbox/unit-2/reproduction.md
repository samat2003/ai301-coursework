# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

samat2003

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Initial full Codex run: 18/20; category floor satisfied; disagreements were `pkg-05` and `pkg-09`.
2. Targeted run `pkg-07,pkg-12`: 2/2.
3. Confirming full Codex run: 18/20. The committed transcript records: `agreement: 18/20 scored items  (bar: 18/20: PASS)`.

The final run's category line is: `categories: clear-accept 6/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

**Package analysis**

`pkg-05`: gold label `accept`; my rubric verdict `reject`. The eval output says: "Report only says it wrote a minimal env.yml with dependencies and category, but does not provide the env.yml contents or the issue's exact URL/input." That fails `steps-followable`: without the YAML contents and exact URL/input, another person cannot recreate the tested state. The final run records `pkg-05  accept  reject   NO     failed: steps-followable`.

**Check rationale**

The `steps-followable` check says: "A stranger can recreate the tested state and perform the trigger from the posted package, including an exact input already shown in the issue and clearly referenced by the report." I kept that requirement because the `pkg-05` eval evidence identifies missing inputs as the reason the attempt cannot be rerun; a plausible result alone does not make the steps reproducible.

**Trade-offs**

The strict input requirement changes the result for `pkg-05`: its gold label is `accept`, while my rubric rejected it because the env.yml contents and exact issue URL/input were omitted. In the targeted `pkg-07,pkg-12` run, both agreed (2/2); the final full run also records `pkg-07  accept  accept   yes` and `pkg-12  accept  accept   yes`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
