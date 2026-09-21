# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60

[The individual Path Review issue page. A link to the repository or the issue list
does not satisfy this field.]

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
Grading Summary

Today's date: 2026-09-21. Repo: codepath/pathreview-ai301-fa26-s3.

Repo-level facts (shared across all three):
- Last push: 2026-09-16 — 5 days ago
- Last 5 commits: all by Aburke225 (non-bot, most recent 2026-09-16)
- Latest release: none (null)
- Archived: no
- CONTRIBUTING.md: not present (policy silent on AI)

House rule applied: classmate "referenced" events do not block an issue.

---

Issue #60 — "Faithfulness checker crashes when a context chunk has text: None"

| Check | Grade | Evidence |
|---|---|---|
| Maintainer alive | pass | Aburke225 committed 2026-09-16 (5 days ago), non-bot account |
| Repo in use | pass | Last push 2026-09-16 — within 30 days |
| Release recency | fail | latestRelease: null — no releases |
| Not already claimed | pass | No assignees, no comments, no linked PRs, no claim activity |
| Scope fits a newcomer | pass | Single file (rag/evaluator/faithfulness_checker.py), steps to reproduce provided, failing test named — no megaissue patterns |
| Contribution policy allows AI use | pass | No CONTRIBUTING.md — policy silent |

Verdict: ACCEPT

---

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Aburke225 committed 2026-09-16 (5 days ago), non-bot, within 30-day threshold"},
      {"name": "Repo in use", "grade": "pass", "evidence": "Last push 2026-09-16T21:50:20Z — 5 days ago, within 30 days"},
      {"name": "Release recency", "grade": "fail", "evidence": "latestRelease: null — repo has never cut a release"},
      {"name": "Not already claimed", "grade": "pass", "evidence": "No assignees, no comments, no linked PRs, no timeline claim events"},
      {"name": "Contribution policy allows AI use", "grade": "pass", "evidence": "No CONTRIBUTING.md found — policy is silent, passes by rule"}
    ],
    "verdict": "accept"
  }
]
```

Note: the skill's own JSON block dropped the "Scope fits a newcomer" row for this
issue (it appears in the readable table above but not the JSON array) — an emission
glitch in the live run, pasted verbatim rather than patched.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. 17/20 — below bar; category floor unmet (`policy 0/1`): my rubric had no check for
   contribution policy at all, so an issue with an outright AI ban still graded accept.
2. 19/20 — bar passed, after adding a "Contribution policy allows AI use" check and
   simplifying the scope check's wording; but this simplification dropped a
   multiplicity requirement, so `issue-09` newly failed "Scope fits a newcomer" (a
   single old closed PR shouldn't read as a track record, but the simplified wording
   let it).
3. 19/20 — bar passed, after restoring the multiplicity requirement; but fixing a
   separate date-anchor bug in "Maintainer alive" (it was measuring 60 days from the
   issue's open date instead of from now) exposed that its non-bot-commit window (14
   days) was too tight, so `issue-06` newly failed on a repo that's genuinely active
   but low-traffic.
4. 20/20 — bar passed, after widening that commit window to 30 days to match "Repo in
   use." This is the run saved to `eval-run.txt` (agreement 20/20 scored items,
   2026-09-21T04:58:05Z).

**Issue analysis**

`issue-12` (bookwyrm-social/bookwyrm#1133): my rubric's first version graded
**accept**; the gold label is **reject** (category: policy). The issue passes every
liveness, scope, and claim check — active repo, unclaimed, a bounded UI fix. But the
repo's CONTRIBUTING.md states "We do not accept AI-generated code or documentation,"
an outright ban. My rubric had no check for contribution policy at all, so it had no
way to see this and defaulted to accept. Adding a required "Contribution policy allows
AI use" check, keyed to the repo's stated policy language, brought this issue to the
correct reject.

**Check rationale**

"Contribution policy allows AI use" — pass condition: "Fail only if the policy is an
explicit outright ban on AI-assisted or AI-generated contributions (e.g. "we do not
accept AI-generated code or documentation"). Pass if the policy is silent, cautions
about review/testing, or conditionally allows AI use." I worded it around an explicit
ban rather than any AI-cautious language because two other bundles in the eval set
have policies that discourage or condition AI use without banning it outright (e.g.
"generative AI tools welcome; you are responsible for reviewing all contributions"),
and those repos are otherwise clear accepts. A stricter trigger would have
false-rejected them.

**Trade-offs**

This threshold passes a repo whose policy is cautious-but-not-a-ban (e.g. one that
"strongly discourages" AI-generated contributions and closes PRs that look untested),
even though that repo's actual tolerance is lower than a silent one. I accept this: in
the eval set, the one bundle with that exact cautious-not-banned language (`issue-10`)
is independently rejected on scope grounds (it's a megaissue tracker), so the looser
policy threshold was never load-bearing for it. I'd rather risk under-flagging a
borderline-cautious policy than false-reject repos that welcome supervised AI use.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit: matches my Python background, and it's a small, single-file fix
   (rag/evaluator/faithfulness_checker.py) that's realistic given the time I have left
   before the deadline — reproducible in a sitting, not a multi-day dig.
2. The verdict correctly identified a live, unclaimed, well-scoped bug with a named
   failing test (`test_none_context_chunk_text`) doubling as the acceptance criterion —
   exactly the liveness-and-scope signal the rubric is built to catch. What I weighed
   beyond that: the actual bug (a `.get(key, default)` call not catching an explicit
   `None` value) is a Python gotcha I specifically want to get comfortable spotting,
   which isn't something any rubric check encodes.
3. Low anticipated difficulty: the issue gives explicit repro steps and names the exact
   test that should pass after the fix, and the change is confined to one method in one
   file.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
