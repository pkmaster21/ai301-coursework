# Evidence guide: where evidence lives in a PR package

An eval package has these sections, in this order: Repo facts, Issue,
Thread highlights, Plan context (the repro evidence, then the plan
with its scope, not-in-scope, files, and test plan), and Candidate PR
(Title, Description, Commits, Diff, Test evidence). The plan is one
paragraph; find its parts by what they say ("Scope", "Not in scope",
"Files:", "Test plan:", "Deliberately deferred", "Known limit"), not by
headings.

In live mode, the same parts live in the student's working copy:
`plan.md` (Diagnosis, Scope, Files to touch, Approach, Test plan,
Risks and unknowns, Deviations), the branch diff from
`git diff main...HEAD`, the commit list from
`git log --oneline main..HEAD`, `test_evidence.md`, and `pr_draft.md`
(title on the first line, the filled PR template below it). The
repo's standards are `.github/PULL_REQUEST_TEMPLATE.md` and
`docs/CONTRIBUTING.md`; the issue and its thread are on GitHub.

## Plan fidelity (harness category: silent-drift)

- Where it lives (eval): the Plan context paragraph's "Scope"
  sentence, its "Files:" list, and its "Not in scope" list, read
  against the Diff's file headers (`--- a/...`) and hunks. Deviation
  or deferral notes appear in the plan ("Deliberately deferred",
  "Known limit") and may be restated in the Description. The
  Description's fidelity claims ("implements the plan exactly", "no
  changes beyond the plan", "no functional changes outside X", "docs
  now document Y") are read against the Diff too.
- Where it lives (live): `plan.md`'s Scope, Files to touch, and
  Deviations sections, read against `git diff main...HEAD`; the
  Summary and Changes sections of `pr_draft.md` read against the same
  diff.
- What good looks like: every hunk maps to a planned change, a test of
  the fix, a required entry (changelog, whatsnew), or a deviation
  note; every planned change is in the diff or disclosed as left out;
  and every claim in the description is true of the diff. calib-01 is
  the shape: one deleted line in the one named file, a description
  that says exactly that. Silent drift in the "more" direction is a
  hunk the plan never mentions (calib-03's rewrite of a file the plan
  never touches, under "no changes beyond the plan"; a new config
  option or flag; a renaming pass; a dependency bump). In the "less"
  direction, it is a planned change missing with no note, or a
  description claiming work the diff does not contain. A shortfall
  that the plan and the description both state (pkg-13's `%VAR%`
  deferral) is honest scope, not drift.

## Test evidence (harness category: not-tested)

- Where it lives (eval): the Candidate PR's Test evidence section
  (and any Before/After block in the Description), read against the
  Plan context's "Test plan:" sentence and the Repro evidence that
  opens the Plan context. Repo checks are named in the test plan and
  in the Repo facts block ("pass the test suite (yarn test)", "cargo
  fmt, clippy, cargo test").
- Where it lives (live): `test_evidence.md` (the four `make` checks'
  output, the repro run on `main` for before and on the branch for
  after), and the Testing section of `pr_draft.md`, read against the
  Test plan section of `plan.md`.
- What good looks like: the plan's own repro, run on the changed code,
  with the observable after-result shown next to the before-result
  and matching the plan's expected-after ("before: TypeError, exit 1;
  after: score = 0.0, exit 0"). Every failing case the test plan names
  is shown, not just one (calib-04 shows repro 1 and is silent on the
  `--exec` abort the plan named). Controls can be reported in a line.
  Each check the plan or repo names appears with its outcome
  ("`cargo test -p ignore` passes (214 tests)"). A failing check
  reported with its reason counts as run. Not good: "tested locally,
  works now", "verified on my machine", "tests pass" with no command,
  or evidence that runs only the control or an unchanged path (a GET
  when the bug is in the POST path).

## Diff quality (harness category: unreviewable)

- Where it lives (eval): the Candidate PR's Diff (every `+` and `-`
  line) and its Commits list.
- Where it lives (live): `git diff main...HEAD` and
  `git log --oneline main..HEAD`.
- What good looks like: every changed line serves the fix, its test,
  or a required entry; comments explain the fix; commit messages say
  what changed. The debris tells are: a debug print or `eprintln!`
  (even commented out); a commented-out first attempt; a dead function
  or variable (an `_unused_...` name, `#[allow(dead_code)]`); a new
  TODO or FIXME; re-indented, re-ordered, or re-wrapped lines the fix
  does not touch; import re-sorting the fix does not need; a hunk in
  an unrelated file. Commit messages like "wip", "fix", or "misc
  cleanups while debugging" are a hint to look harder at the diff.

## Standards and comms (harness category: standards-wall)

- Where it lives (eval): the Repo facts block's "pull requests" line
  (template sections, checklist items, issue-link form, changelog or
  whatsnew asks, title convention) and "contribution policy" line
  (AI-use policy), plus the Thread highlights for maintainer
  direction; read against the Title, the Description, and the Diff
  (a required changelog entry has to be in the Diff).
- Where it lives (live): `.github/PULL_REQUEST_TEMPLATE.md` (Summary,
  Issue with `Closes #`, Changes, Testing checklist, Screenshots /
  Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md` (conventional
  commits, the five checks, removing the issue's xfail marker), read
  against `pr_draft.md`. The course requires an AI-use disclosure on
  every PR, in Notes for Reviewers. Maintainer direction is in the
  issue thread on GitHub.
- What good looks like: every template section has real content, the
  issue link is in the asked form (`Closes #60`), each checklist item
  is met or addressed, only true boxes are ticked (the CI box stays
  unticked before the PR is opened), and when policy requires it, the
  description discloses AI use in a way that meets what that policy
  asks: pkg-11's "this change was AI-assisted with me reviewing and
  understanding every line, and this text is in my own words" meets a
  policy that asks for a human in the loop and own-words comments;
  a policy like pkg-20's that asks for the tool and the extent needs
  both named. Explicit maintainer direction
  is followed or answered (pkg-05 engages the maintainer's uniqueness
  question). Not good: a required checklist, issue link, or whatsnew
  entry visibly missing (pkg-01), or no AI disclosure under a policy
  that requires it (pkg-20). Whether the description's claims match
  the diff is plan fidelity, above.
