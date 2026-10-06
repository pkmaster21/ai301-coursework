# Evidence guide: where evidence lives in a plan package

An eval package has these sections, in this order: Repo facts, Issue,
Thread highlights, Repro evidence, Candidate plan, Candidate plan
comment. The plan itself may or may not use headings; look for its
parts by what they say ("Cause:", "Change:", "In:", "Out:", "Test:",
"Risks:"), not by formatting.

## Diagnosis and grounding

- Where it lives (eval): the cause is usually the first part of the
  Candidate plan ("Cause:", "Diagnosis:", or the opening sentence).
  The behavior it must explain is in the Repro evidence section's
  numbered steps, "Expected" and "Actual" lines, and especially any
  control run (the same steps with one thing changed: a flag removed,
  a feature turned off, a component taken out of the loop, a timing
  or output matrix).
- Where it lives (live): the Diagnosis section of plan.md, read against
  the student's posted repro comment on the issue and the repro output
  quoted in plan.md.
- What good looks like: the stated cause is a specific mechanism (a
  function, a code path, a missing refresh, a wrong value) and every
  repro step and control is consistent with it. In calib-01, "the
  commits view's model is not refreshed after push" fits step 4 (the
  color fixes itself on re-entry). A diagnosis fails when a control
  rules it out: if the blamed component is absent and the bug still
  happens, the cause is somewhere else. A cause copied from the
  thread does not count as grounded if the repro evidence contradicts
  it.

## Scope

- Where it lives (eval): the Candidate plan's change list ("Change:",
  "In:", numbered changes) and its not-in-scope line ("Out:", "Not in
  scope", "won't touch", "defer"). Measure it against the Issue
  section's one reported behavior.
- Where it lives (live): the Scope and Files sections of plan.md,
  read against the issue body.
- What good looks like: one bounded change at named files that makes
  the reproduced behavior correct, plus a line saying what will be
  left alone. A plan that defers a bigger rework with a reason is
  still bounded. It is not bounded if the fix is bundled with
  while-I'm-here work: dependency or framework migrations, upgrades,
  new options or settings, UI rework, CI matrix changes, test-harness
  migrations, or a rewrite of the surrounding component. The named
  files do not need to appear in the repro evidence. A specific code
  path in a named module counts as a named location (pkg-14's "the
  client attach/reattach path in `zellij-server`") even without a
  file path. Deferring a variant the author cannot test, with a
  reason, is bounded scope, not a gap.

## Executability

- Where it lives (eval): the Candidate plan's change and approach
  lines, and the file paths or function names they mention.
- Where it lives (live): the Files and Approach sections of plan.md.
- What good looks like: a stranger could open the named file and start
  the edit without asking anything. There is one chosen approach and
  it says where it goes ("the push completion callback in
  `pkg/gui/controllers/sync_controller.go`"). Pinning the exact
  function during the build is fine once the approach and the code
  path are chosen. It fails if the plan
  says to investigate or profile first, puts the fix "somewhere",
  lists candidate layers without picking one, or leaves a core choice
  open ("upstream or vendored, whichever is easier").

## Test plan

- Where it lives (eval): the "Test:" or test-plan part of the
  Candidate plan, read against the Repro evidence steps.
- Where it lives (live): the Test plan section of plan.md, read against
  the student's repro steps.
- What good looks like: it re-runs the repro (or a regression test
  built from it) and names the exact observable result after the fix,
  tied to a repro step ("at step 3 the color must flip without leaving
  the view", "exit 0", "the three spellings all match"). That result
  would still be wrong if the stated cause were still there. It fails
  if the only test is "run the full suite", or the outcome is vague
  ("should feel fast", "nothing else should feel broken"), or it
  checks a symptom the diagnosis doesn't explain. Automation is not
  required.

## Honesty

- Where it lives (eval): any "Risks", "Unknowns", "Not sure", or
  "will check" lines in the Candidate plan, and how certain the
  cause is worded compared with what the repro evidence shows.
- Where it lives (live): the Risks and unknowns section and the
  Deviations section of plan.md. A deviation made during the build is
  recorded under Deviations, and in a follow-up issue comment if the
  posted plan is no longer true.
- What good looks like: specific unknowns with a way to check them
  ("tool-by-tool `--` support is a checked unknown"). An unknown is
  fine. An unknown that blocks the chosen approach means the plan is
  not executable yet. Confident wording about a cause the evidence
  does not support is false confidence, not honesty.

## Comms

- Where it lives (eval): the Candidate plan comment, read against the
  Thread highlights (comments from OWNER, MEMBER, CONTRIBUTOR, or
  COLLABORATOR accounts) and the Repo facts block's "contribution
  policy" line.
- Where it lives (live): comment.md, read against the issue thread on
  GitHub and the repo's CONTRIBUTING, AI policy, and PR/issue
  templates.
- What good looks like: when a maintainer has named a culprit,
  preferred approach, or request in the thread, the comment follows
  it or says why it doesn't. When the repo policy requires AI-use
  disclosure, the comment states the tool used and the extent of the
  help. It fails if it proposes something the maintainer's direction
  points away from without saying so (for example, a docs-only
  workaround after the owner isolated the culprit file and asked for
  testing), or if it leaves out a required disclosure. With no
  maintainer direction and no stated policy, this passes by default.
