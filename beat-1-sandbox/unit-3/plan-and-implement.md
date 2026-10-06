# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

pkmaster21

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-6025724442

Plan for the fix, based on my repro above (the `TypeError` at `faithfulness_checker.py:38` from `FaithfulnessChecker().check('Knows Python.', [{'text': None}])`).

**Cause:** `chunk.get("text", "")` only falls back to `""` when the `text` key is missing. With `{"text": None}` the key is present, so `.get()` returns `None` and `" ".join(...)` can't join it. As a control I ran `test_missing_text_key_in_chunk` (no `text` key) on the same unchanged commit `2f4e82f`: `1 passed`, and the issue's snippet with `[{'content': 'Python skills'}]` prints `score = 0.0`. So the join is fine when it gets strings.

**Change:** on line 38, use `chunk.get("text") or ""` so a `None` value falls back to `""` the same way a missing key already does. This is the same one-line fix sid-pandya proposed above; my plan is grounded in my own repro and control above, and I'm building it on my own branch, `fix/60-none-context-chunk-text`. Following CONTRIBUTING's seeded-bug section, I'll also remove the issue #60 `xfail(strict=True)` marker from `test_none_context_chunk_text`.

**Not changing:** the claim-scoring logic, the caller in `eval_suite.py`, non-string `text` values like ints, or the issue #59 xfail tests in the same file.

**Test:** re-run my repro. `test_none_context_chunk_text` should go from failing with the `TypeError` to `1 passed` without `--runxfail`, and the issue's snippet should print `score = 0.0` with exit 0 instead of raising. I'll also run the missing-key snippet as a control (should still be `score = 0.0`) and the full checker test file (should pass, with only the #59 xfails left).

**Open question I'll check:** `or ""` also maps other falsy values to `""`, and a non-string truthy value would still raise. I'll look at what `eval_suite.py` passes in and mention it in the PR if it can send anything other than strings or `None`.

I'll post the before/after output when the branch is ready.

---

## Your branch

**Branch**

fix/60-none-context-chunk-text

**Evidence**

Before: my Unit 2 repro steps on unchanged `main` (`2f4e82f`). `--runxfail` is needed here because the test was still marked `xfail` for this issue.

```
== commit: 2f4e82f (main, unchanged)
$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text --runxfail
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text FAILED [100%]
>       context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
E       TypeError: sequence item 0: expected str instance, NoneType found
rag/evaluator/faithfulness_checker.py:38: TypeError
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
======================= 1 failed, 21 deselected in 0.17s =======================

$ .venv/bin/python -c "...check('Knows Python.', [{'text': None}])"
  File "pathreview-ai301-fa26-s3/rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
TypeError: sequence item 0: expected str instance, NoneType found
exit 1

$ .venv/bin/python -c "...check('Knows Python.', [{'content': 'Python skills'}])"
2026-10-06 17:24:50 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
score = 0.0
exit 0

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -m unit
======================== 18 passed, 4 xfailed in 0.15s =========================
```

After: the same steps on my branch. The `xfail` marker is gone, so the named test runs without `--runxfail`.

```
== branch: fix/60-none-context-chunk-text (commit 619114e)
$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text PASSED [100%]
======================= 1 passed, 21 deselected in 0.21s =======================

$ .venv/bin/python -c "...check('Knows Python.', [{'text': None}])"
2026-10-06 17:26:14 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
score = 0.0
exit 0

$ .venv/bin/python -c "...check('Knows Python.', [{'content': 'Python skills'}])"
2026-10-06 17:26:14 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
score = 0.0
exit 0

$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -m unit
======================== 19 passed, 3 xfailed in 0.24s =========================

$ .venv/bin/python -m pytest tests/unit -m unit -q
376 passed, 52 xfailed, 1 warning in 9.66s
```

The named test went from `1 failed` with the `TypeError` at line 38 to `1 passed`, and the issue's snippet went from exit 1 to `score = 0.0` with exit 0. The missing-key control gave `score = 0.0` both times, so the path that already worked is unchanged.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 19/20, bar passed, every category matched except one miss in clear-accept (6/7). `pkg-14` (clear-accept) was rejected on `within-issue-scope` and `stranger-could-start`.
2. Partial run, `--only pkg-14,pkg-10,pkg-17,pkg-18,pkg-06,calib-04 --include-calibration`: 5/5 scored items agree (`pkg-14` now accepts, the canaries still reject), and `calib-04` still rejects. Partial runs print no bar verdict.
3. Full run: 20/20, bar passed, every category matched. This is the run saved to `eval-run.txt` ("agreement: 20/20 scored items  (bar: 18/20: PASS)").

**Package analysis**

`pkg-14` (zellij-org/zellij#5174): on my first full run my rubric graded it **reject**, and the gold label is **accept** (category: clear-accept). The plan says its fix goes in "the client attach/reattach path in `zellij-server` (session connection handling)", with "exact functions to be pinned in the PR after tracing the query issuance with debug logs." My `within-issue-scope` check required the plan to name "the file(s) it will change", so a code path inside a named crate didn't count. My `stranger-could-start` check failed it for leaving a decision open, because "exact functions to be pinned" read as deferring the key choice to build time. But the plan had already chosen its approach ("consuming or draining pending OSC query responses in the client attach path ... before pane input is wired") and its code path. Only the line-level location was still open, and a stranger could start from that. I revised both checks so a specific code path within a named module counts as a named location, and so pinning the exact function during the build is fine once the approach and code path are chosen. After that, `pkg-14` accepted.

**Check rationale**

From `tools/plan-check/rubric.md`, exactly as it reads now:

| within-issue-scope | The candidate plan's scope and files-to-change statements, read against the Issue and the Repro evidence. | Passes if the plan names where it will change things (specific files, or one specific code path within a named module or package, such as "the reattach path in `zellij-server`'s client connection handling"), says what it will not change, and every planned change is needed to fix the reproduced bug. The named locations do not have to appear in the repro evidence. Fails if no location is named or the location is left open ("somewhere", "the codebase", a list of candidates with none chosen), or if the plan bundles work the issue never asked for (drive-by refactors, dependency or framework migrations, new options or settings, UI rework, or a redesign around the fix). | required |

It started as my group's worksheet check: "passes if the section mentions files it will change and what it won't change as well; fails if no specific files are mentioned." In the class activity it held calib-01 because we also required the named file to appear in the repro evidence, so I dropped that requirement ("The named locations do not have to appear in the repro evidence"). I added the scope-creep half (the list of drive-by refactors, migrations, new options, UI rework, and redesigns) because the original check only looked at whether files were named, and all four scope-creep packages name files. Last, after `pkg-14`, I widened "names the file(s)" to "specific files, or one specific code path within a named module or package". I kept "the location is left open ("somewhere", "the codebase", a list of candidates with none chosen)" as a fail, so the widening couldn't let through plans that never pick a place.

**Trade-offs**

Loosening the location requirement for `pkg-14` gives up some strictness: a plan that names only a module-level code path and never a file can now pass `within-issue-scope` and `stranger-could-start`. The packages most at risk of flipping were the ones those two checks catch, so before spending a full run I re-ran them as canaries with `--only`: all three unbuildable packages (`pkg-10`, `pkg-17`, `pkg-18`, which fail on "no files", "gocui? tcell? not sure", and recover() "somewhere"), one scope-creep package (`pkg-06`), and `calib-04` as a free trap. All of them still rejected, and the confirming full run showed every category at full agreement.

One case I accept the skill will miss: the procedure doesn't say how to treat classmates' plans on a shared issue, and it doesn't check whether a run the plan cites was actually done by the author. When I graded my own plan, the skill flagged that my comment cited the missing-key control as if I'd run it, but it couldn't fail it, because no check covers that. I ran the control and added its output before posting. I didn't add a check for it, because it would change the files the 20/20 run fingerprints and none of the eval packages test for it.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
