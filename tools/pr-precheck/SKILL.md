---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this ready to
submit?** A PR package is a candidate pull request (its title, its
description, its commits, its diff, and its test evidence), read
against three things: the plan the PR claims to implement (including
any deviation notes), the issue that plan belongs to, and the repo's
stated standards (PR template, contributing guide, AI-use policy).

Do not answer any other question. Do not review code style for its
own sake, do not judge whether the plan itself was a good plan (that
was week 3), and do not suggest a better fix. Grade exactly one
package per run, and grade it only by executing the components in
this directory.

## Inputs and modes

Decide the mode first. If the prompt hands you a package bundle (a
markdown file headed `# Eval package:`) or says EVAL MODE, you are in
eval mode. If the prompt asks you to grade the student's own draft PR
from a working copy, you are in live mode.

**Live mode** (the student's own PR, before it is opened). Read these
inputs, all from the student's working copy of their fork unless
named otherwise:

1. `scope.md` and `voice-guide.md` in this skill directory (see the
   two seam sections below).
2. The plan: `plan.md` at the top of the working copy, including its
   `## Deviations` section. A house-chain student reads the house plan
   and the house repro pack the instructor gave them instead; the same
   checks grade the same things there.
3. The diff: run `git diff main...HEAD` (three dots) from the working
   copy. This is every committed change the branch makes relative to
   `main`. Also run `git log --oneline main..HEAD` for the commit list
   and `git branch --show-current` for the branch name. Uncommitted
   changes are not part of the PR; ignore them, and say so if
   `git status` shows code files with uncommitted edits.
4. The draft title and description: `pr_draft.md`. Its first line is
   the title; everything after it is the description.
5. The test evidence: `test_evidence.md`.
6. The issue: the issue URL the student gives you. Read the issue body
   and its full comment thread with `gh issue view <number> --repo
   <scoped repo> --comments` (or the GitHub web page if `gh` fails).
7. The repo's stated standards: `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md` in the working copy. The course also requires
   an AI-use disclosure on every PR; treat that as a stated policy in
   live mode, and expect it in the template's Notes for Reviewers
   section.

The package is what those files and that diff contain. Do not grade
other files in the working directory, and do not count anything the
student tells you in chat as evidence.

Refuse to grade in live mode, and say which input is missing, if
`plan.md`, `pr_draft.md`, or `test_evidence.md` does not exist, or if
`git diff main...HEAD` is empty (there is no change to grade).

**Eval mode** (a frozen practice submission). The bundle is the whole
world. Every fact comes from the bundle text: its repo-facts block,
issue, thread highlights, plan context, and candidate PR (title,
description, commits, diff, test evidence). Fetch nothing, run no
commands against any repo, and read no other files. Eval mode always
grades a complete package: every check in the rubric, and the full
verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Take the repo
named on its `Repo:` line as the only repo you may grade PRs for, and
apply its house rules (own fork's branch, one PR per issue, template
always used, a classmate's PR does not block this one) when you read
the evidence.

- If the `Repo:` line still holds the placeholder `<ORG>/<PATH-REVIEW-REPO>`
  (or any bracketed placeholder), stop without grading. Tell the
  student to fill the `Repo:` line in `scope.md` with their section's
  Path Review repo. Do not emit a verdict.
- If the issue URL, or the working copy's `origin`/`upstream` remote,
  points at a different repo than the scope names, refuse to grade and
  say which repo the scope allows. Never guess a scope and never widen
  it.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after `scope.md`. Hold the
outgoing PR text, the title and the description in `pr_draft.md`,
against each rule in it. In the summary, list every rule the draft
breaks: quote the rule, quote the line of the draft that breaks it,
and say what to change. If the draft breaks none, say so in one line.

The voice guide is the student's own style, not a standard the repo
sets, so it never changes the verdict on its own. Only a rubric check
can change the verdict. In eval mode, ignore `voice-guide.md`
entirely.

## Component reads

Read every component before grading anything:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It decides what passes.
- `references/evidence-guide.md` is the map: for each evidence family,
  where it lives in an eval bundle and in a live working copy, and
  what good looks like. Use it to locate every piece of evidence a
  check names.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, verdict assembly. Execute it as written,
  step by step.

If the procedure is silent on something you need (for example, where
to look for a piece of evidence it never mentions), do the smallest
reasonable thing, and name the gap in your summary as "procedure gap:
..." so the student can fix it. Never invent a new check, and never
skip a check because its evidence is hard to find.

If `rubric.md` has no checks or no verdict rule, or `procedure.md` has
no steps, stop and say so: this tool cannot grade without a rubric and
a procedure, and an empty tool that invents checks at runtime would
produce verdicts that look like judgment and are noise. Do not emit a
verdict in that case.

## Verdict and output

The verdict is binary: `accept` means ready to submit (open the PR),
and `reject` means hold (fix something first). There is no third
verdict and no score. Put reservations in a check's evidence line, not
in the verdict.

Before the JSON block, print a short readable summary: one line per
check (name, grade, the deciding fact), the deciding check for the
verdict, and in live mode the voice-guide notes and a short list of
what to fix. Then end your reply with this fenced JSON block, with one
entry per rubric check in rubric order. The block must be valid JSON,
and nothing may follow it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In eval mode, `item` is the bundle id the prompt gives you. In live
mode, the PR is not open yet, so use the issue URL followed by the
branch name (for example `.../issues/60 @ fix/60-none-context-chunk-text`).

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it: a file in the diff, a line of the
  description, a line of test output, a template section. "Looks fine"
  is not evidence.
- **Grade the thing, not the polish.** Read the diff itself against
  the plan, the evidence itself against the test plan, and the
  description against the diff. A terse PR can be ready, and a
  polished, confident one can be hiding drift. Never take the
  description's word for what the diff contains; check the diff.
- **The rubric decides, not you.** If a check passes by its stated
  pass condition but feels wrong, it still passes. Note the tension in
  the summary; the fix belongs in the rubric.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear follows the verdict rule.** Apply `unclear` as the
  rubric's verdict rule says. Where the rule is silent, treat
  `unclear` as `fail`: a claim you cannot verify from the package is a
  failing one, and a PR you cannot verify is not ready to submit.
- **Honest shortfalls are not failures.** A PR that does less than
  everything, and says so in its plan's deviation notes and its
  description, can be ready. Hold a PR for hiding a gap, not for
  disclosing one.
