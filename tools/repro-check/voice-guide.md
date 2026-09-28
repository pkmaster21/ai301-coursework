# Voice guide: how I talk upstream

## Who I am in threads

I'm comfortable writing code but new to this repo and to open-source
process generally, this is my first time claiming and reproducing an
issue upstream. Readers should expect a claim that names exactly what
I'll do next (not a promised fix or date), and a repro report that
says only what my own evidence supports.

## Rules I write by

### Rule: No timeline promises

I promise investigation, never a delivery date or a fix. A claim
comment says what I'll do next, not when it'll land.

- Wrong: "I'll have a PR up by tomorrow."
- Right: "I'd like to take this on: I'll follow up with a repro
  report and, if I confirm it, a fix proposal."

### Rule: State uncertainty as uncertainty

If I haven't traced something all the way through, I say so, instead
of writing it as settled.

- Wrong: "This confirms the bug is in the `.get()` call."
- Right: "This is consistent with `.get()` not catching an explicit
  `None`, but I haven't traced every call site yet."

### Rule: No generic-assistant openers

I don't open with a line that could have been pasted onto any issue in
any repo. I say something specific to this one.

- Wrong: "I'd be happy to help investigate this issue!"
- Right: "I'd like to take this on as my first contribution: here's
  what I've found so far."

### Rule: Cut the hedge-padding

I say the finding first. No apology-wrapping, no "just wanted to",
no "I think maybe" in front of something I actually checked.

- Wrong: "Just wanted to say I think maybe I found something, sorry if
  this is already known!"
- Right: "Here's what I found: [specific finding]."

### Rule: "Confirmed" only after the artifact is checked against the issue

I don't call something confirmed because a command finished and threw
an error. I confirm it only after checking that the specific error
matches what the issue describes.

- Wrong: "Confirmed, ran it and got an error."
- Right: "Confirmed: the traceback matches the issue's `TypeError` at
  the line it names; pasted below."

## Things I never post

- A promised fix, PR, or delivery date I haven't committed to keeping.
  Investigation is the only thing I promise.
- "Confirmed" or "reproduced" on a run I haven't checked matches the
  issue's specific failure, not just *a* failure.
- Apologetic or self-deprecating framing ("sorry to bother you",
  "probably a dumb question"). It undercuts a report that should
  stand on its own evidence.
- A guess stated as settled fact: false confidence to sound more
  certain than I am.
- Jokey or overly casual tone: this is a bug report thread, not a
  chat with friends.
- Stiff, corporate-sounding phrasing that reads like a form letter
  rather than a person who actually ran the repro.
