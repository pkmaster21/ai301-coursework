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

### Rule: A plan comment names the fix, the files, and the test

A plan comment is not a restatement of the bug. It says what I'll
change, where, what I'm leaving alone, and what result will show it
worked, all tied back to my repro.

- Wrong: "I'll look into fixing this, should be straightforward."
- Right: "Plan: add a None check in `core/parser.py`'s `load()`; I'm
  not touching the CLI wrapper. Re-running my repro step 2 should
  print the default instead of the `TypeError`."

### Rule: Answer the thread before proposing my own path

If a maintainer has already pointed at a cause or asked for something,
my plan comment says how my plan lines up with it, or why it doesn't.

- Wrong: "My plan is to rewrite the handler." (ignoring a maintainer
  comment that named a different file)
- Right: "This follows the direction above to fix it in
  `light_windows.go`; I'm leaving the docs alone."

### Rule: A PR title says what the change does

The title is the one line a maintainer reads before deciding to open
the PR. It names the behavior that changes, in the repo's commit
format, not how I feel about it.

- Wrong: "Fixed the bug!!"
- Right: "fix(rag): handle None text in faithfulness checker context
  chunks"

### Rule: The description promises exactly what the diff contains

Every change I list is in the diff, and everything in the diff is
listed. I don't write "exactly as planned" unless I've checked the
diff against the plan.

- Wrong: "Implements the plan exactly, plus a few small cleanups."
- Right: "Changes: one expression on line 38 of
  `faithfulness_checker.py`; removed the issue #60 xfail marker."

### Rule: A shortfall is stated plainly, not apologized for

If I left something out or a check failed, I say what, and why, in
one or two sentences, so a reviewer can weigh it.

- Wrong: "Sorry, I couldn't get integration tests working, hope
  that's ok."
- Right: "`make test-integration` reports 'no tests ran': the repo
  has no integration tests yet. Output pasted below."

### Rule: AI use is disclosed in my own words

I say which tool I used and what it did on this change, and that I
reviewed every line I'm submitting.

- Wrong: (no mention)
- Right: "I used Claude Code to help trace the bug and draft this
  description; I wrote and reviewed the one-line fix and ran every
  check myself."

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
- A ticked checkbox or a "tests pass" claim that my pasted evidence
  doesn't show. Claiming coverage I didn't run is worse than leaving
  the box empty.
