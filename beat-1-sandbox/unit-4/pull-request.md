# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/219

**Branch**

`fix/60-none-context-chunk-text`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 18/20, bar passed, but clear-accept was 5/7. `pkg-05` was rejected on `repro-before-after` and `pkg-08` on `repo-checks-run` (this run was not saved).
2. Partial run, `--only pkg-05,pkg-08,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`: 6/6 scored items agree (`pkg-05` and `pkg-08` now accept, the four not-tested canaries still reject), and `calib-04` still rejects. Partial runs print no bar verdict.
3. Full run: 19/20, bar passed, clear-accept 6/7. `pkg-11` was rejected on `policy-and-thread`.
4. Partial run, `--only pkg-11,pkg-20,pkg-01`: 3/3 agree (`pkg-11` now accepts, both standards-wall packages still reject).
5. Full run: 20/20, bar passed, every category matched. This is the run saved to `eval-run.txt` ("agreement: 20/20 scored items  (bar: 18/20: PASS)").

**Package analysis**

`pkg-05` (nushell/nushell#18848). Gold label: accept. My rubric rejected it on the first full run, failing `repro-before-after`. The plan's test plan names two cases: re-run the issue's script and "expect both `atuin` rows", and "re-run with two same-name same-key bindings, expect one row plus a warning". The PR shows the first case as a before/after table, but reports the second in one sentence: "Same-key redefine prints the one-time warning." My first rubric said "When the plan's test plan names more than one failing repro or failure mode, each one must be shown with its after-result", so the grader treated the warning case as a failure mode with no output shown. But that case is not the bug the issue reports; it checks new behavior the fix adds. I split the test plan's cases into failing cases from the repro (shown with output) and other cases (controls and new-behavior checks, which only need a reported result). After that change, `pkg-05` accepts, and `calib-04`, whose evidence is silent on a second failure mode the issue reports, still rejects.

**Check rationale**

From `tools/pr-precheck/rubric.md`, the `repro-before-after` pass condition reads in part:

> Every failing case from the repro evidence (the bug the issue reports, in each failure mode the plan names) must be shown with its after-result. Other cases in the test plan (controls that already worked, extra cases for new behavior the fix adds such as a warning) only need their result reported in a sentence; staying silent on one fails.

It reads this way because my first version, "each one must be shown with its after-result", made no difference between the bug and the extra cases around it, and it rejected `pkg-05`, an honest accept. I rejected the simpler fix of only requiring the main repro, because that would let `calib-04` through: its plan names two failure modes from the issue and the evidence only re-runs one. Keeping "staying silent on one fails" means a PR can summarize a control in a sentence but cannot skip a case the plan promised to test.

**Trade-offs**

Loosening `repro-before-after` and `repo-checks-run` could have let a not-tested package through, so I re-ran every not-tested package (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) plus `calib-04` as canaries with `--only`, and all five still rejected. The trade-off I accept: `repro-before-after` now trusts a one-sentence report for controls and new-behavior cases, so a PR that falsely claims "control unchanged" without running it would pass that check. Likewise, loosening `policy-and-thread` to accept a disclosure that meets only what the repo's policy asks was checked with `pkg-20` and `pkg-01` as canaries, and both still rejected.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
