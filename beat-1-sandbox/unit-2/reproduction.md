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

pkmaster21

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5755757562

Hi! I'd like to take this on as my first contribution. The issue's diagnosis matches
what I see in the code: `check()` reads context with `chunk.get("text", "")`, but
`.get()`'s default only applies when the key is *missing*, not when it's present with
value `None`, so an explicit `text: None` slips through and the later `" ".join(...)`
call raises `TypeError`. I'll reproduce this against `test_none_context_chunk_text` and
follow up with a repro report; if it checks out, I'll look at whether the fix belongs in
how `check()` reads the chunk or elsewhere.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5863732145

Confirmed: the traceback matches the issue's `TypeError` at the line it names.

Environment: Python 3.14.4, macOS 27.0 (arm64), repo at `main` commit `2f4e82f`, fresh
venv via `python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"` (no Docker/DB
needed, this bug is pure Python and never touches the database).

Steps, from a clean clone:

```
$ git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
$ cd pathreview-ai301-fa26-s3 && git checkout 2f4e82f
$ python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"
$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text --runxfail
```

(`--runxfail` matters here: the test is already marked `xfail` for this issue, so
without that flag pytest reports `XFAIL` instead of showing the traceback.)

Actual:

```
context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
TypeError: sequence item 0: expected str instance, NoneType found
```
at `rag/evaluator/faithfulness_checker.py:38`, exact same message and line the issue
reports.

Also reproduced with the issue's own snippet:

```
$ .venv/bin/python -c "
from rag.evaluator.faithfulness_checker import FaithfulnessChecker
FaithfulnessChecker().check('Knows Python.', [{'text': None}])
"
```
produces the identical `TypeError`.

Expected: per the existing `test_none_context_chunk_text` test (already marked `xfail`
for this issue), the checker should handle a `text: None` chunk gracefully and return a
float score, the same way `test_missing_text_key_in_chunk` already handles a chunk with
no `text` key at all.

Root cause: `.get("text", "")`'s default only applies when `"text"` is missing from the
dict, not when it is present with value `None`, so an explicit `None` slips through into
`" ".join(...)`, which cannot join a `NoneType`.

If this diagnosis holds up under review, I'll propose a fix, likely normalizing `None`
to `""` at this line, mirroring how the missing-key case is already handled correctly.

## Eval iterations

**Run history**

1. 18/20: bar passed, but two clear-accepts were missed: `pkg-01` failed "Replicatable"
   because that check's wording assumed a clone/checkout/build/run shape, penalizing an
   already-installed CLI's offline repro for having no build step it never needed;
   `pkg-10` failed "Behavior matches the issue" because that check required the artifact
   to show the issue's specific failure even on an honest cannot-reproduce, which by
   definition won't show that failure.
2. Revised both checks (loosened "Replicatable" to accept an already-installed
   tool/version plus a command as a valid starting point; added a cannot-reproduce
   carve-out to "Behavior matches the issue"), confirmed with a `--only` canary
   (`pkg-01,pkg-10,pkg-02,pkg-04,pkg-06`, 5/5 agree) before spending a second full run.
3. 19/20: `pkg-01` and `pkg-10` now correctly accept, but `pkg-19` newly flipped to
   accept: the loosened "Replicatable" wording that fixed `pkg-01` also removed the
   accidental cover it had been giving `pkg-19`, whose real problem was an
   over-promising, boilerplate claim comment that no check evaluated. Added a new
   required check, "Claim is specific, not boilerplate," confirmed with a `--only`
   canary (`pkg-19,pkg-20,pkg-01,pkg-05,pkg-10`, 5/5 agree) before the confirming run.
4. 20/20: bar passed, every category matched (agreement 20/20 scored items,
   2026-09-28T04:38:02Z).
5. 20/20 after review feedback: moved the edge-case detail (installed-tool starting
   point, minimal `env.yml` example, cannot-reproduce carve-out) out of the rubric rows
   so `references/evidence-guide.md` is its single home, renamed "Replicatable" to
   "Reproducible steps", and stripped template scaffolding. Re-ran to confirm the
   rewording changed no verdicts; every category still matched. This is the run saved to
   `eval-run.txt` (agreement 20/20 scored items, 2026-10-01T00:17:01Z).

**Package analysis**

`pkg-19` (vuejs/core#15205): my rubric's step-2 revision graded **accept**; the gold
label is **reject** (category: unfollowable-comms). The repro report itself is genuinely
solid: environment pinned, steps followable through the SFC playground, and the
produced CSS output matches the issue's described unscoped-selector bug exactly. But the
claim comment reads "Kindly assign it to me, I will fix it within 2 days guaranteed...
keep this issue reserved for me," interchangeable boilerplate promising a guaranteed
fix before any investigation. My rubric at that point had no check that read the claim
comment's own content at all; my checks covered environment, steps, behavior, honesty,
and disclosure, but nothing judged the claim's specificity or its promises. A good repro
report attached to a bad claim comment defaulted to accept. Adding a required "Claim is
specific, not boilerplate" check, keyed to whether the claim names something specific to
this issue and promises investigation only, brought this issue to the correct reject.

**Check rationale**

"Claim is specific, not boilerplate," pass condition: "Pass if the claim names
something specific to this issue that couldn't be pasted unchanged onto a different one,
and promises investigation only, no guaranteed fix, no delivery date. Fail if the
comment is generic 'assign me' boilerplate, or promises a specific fix/timeline before
the investigation backing it exists." I considered merging this with "Conventions/
disclosure respected" into one combined "Comms" check to cut the rubric from 7 checks to
5 and speed up grading, but rejected that: it repeats the exact bundling problem flagged
in my Unit 1 feedback, where packing multiple independent conditions into one row means
a fail forces you to re-read the whole condition to find which sub-condition tripped it.
Keeping this check separate from disclosure means a fail here always means one specific
thing: the claim itself is generic or over-promising, independent of whether the repo
even requires disclosure.

**Trade-offs**

Loosening "Replicatable" to accept an already-installed tool/version plus a command
(instead of requiring a clone/checkout/build/run shape) trades away the ability to catch
a report that quietly skips an install or setup step it should have shown. I accept this
because canary `pkg-06` (unfollowable-comms: no environment record, and steps that omit
the driver on a Windows-specific issue) still correctly failed after the change, on the
same missing-step logic: the loosening didn't buy back the exact failure mode the check
exists to catch, it only stopped punishing packages like `pkg-01`'s offline HTTPie repro
and `pkg-05`'s minimal `env.yml` repro, which never needed a build step in the first
place.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
