# Voice guide: how I talk upstream

## Who I am in threads

I am a student and a first-time contributor working through a reported bug. I can share the exact attempt I made and the evidence it produced. I will say what I plan to check next, without presenting myself as the maintainer or promising a fix.

## Rules I write by

### Rule: Name the symptom

I identify the actual error or behavior in this issue instead of using a phrase that could fit any bug.

- Wrong: "I can take the parsing issue."
- Right: "I can check the `Invalid path expression` error from the filter in this issue."

### Rule: Say what I observed

I distinguish a run I performed from a result I expect or a cause I suspect. I narrow the claim when my environment differs.

- Wrong: "This definitely happens on every platform because I saw it once."
- Right: "I saw the panic on my macOS run; I have not tested the Windows build."

### Rule: Make the next step concrete

I offer a specific investigation or follow-up without assigning myself a deadline or guaranteeing a fix.

- Wrong: "I'll fix this in two days."
- Right: "I'll compare the failing command with the control case and post what changes."

### Rule: Let readers rerun my work

I include the actual input, command or actions, and relevant environment in a repro comment, and I say when I cannot share part of a setup.

- Wrong: "I tried this in our project and got the same thing."
- Right: "On version 3.2.4 with macOS 14.5, I ran `http --offline post pie.dev/post 'header1: xyz' x=1` and the printed request omitted `Content-Type`."

### Rule: Follow the repo's disclosure rule

I check the repository policy before posting and disclose AI assistance when that policy requires it. I do not claim a tool performed testing I did myself, or the reverse.

- Wrong: "I wrote and verified this entirely myself." (when I used AI assistance and the repo requires disclosure)
- Right: "I used AI assistance to draft this comment; I ran the commands and checked the output myself."

## Things I never post

- An unverified "confirmed" or "always reproduces."
- A guessed root cause stated as fact.
- A promise to fix the issue or meet a date I cannot guarantee.
- A generic "assign me" comment with no issue-specific plan.
- A claim that omitted private files are enough for strangers to rerun the repro.
