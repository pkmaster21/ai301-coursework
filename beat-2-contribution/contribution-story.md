# Contribution: Studio: remove dead code and a developer-local path

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** David Zhao
**GitHub Username:** pkmaster21
**Issue:** https://github.com/AlphaWaveSystems/flutter-probe/issues/401
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my issue-select skill into a scout that searches all of GitHub**

I wrote four `gh search issues` queries from my fit profile and eight
ranking rules, then ran one scan that graded the top five of 387
results. Three of the five were already claimed through a comment or
an open linked PR that `--no-assignee` can't see, so I added
`-linked:pr` to every query. The one strong-looking accept,
code-charity/youtube#2600, turned out to be from 2024 with no
maintainer reply ever; a cross-link comment had bumped its update date
past my 30-day filter, so I dropped it and now check myself that a
maintainer opened or answered an issue before trusting a pick.

**Unit 6: Installed the voice guard and wrote a memory file for flutter-probe**

I added an Upstream section to my voice guide (claims, questions,
pings) and installed the guard as a hook, and it did its job: it
stopped one version of my claim that had lost its check-in line, quoting
my own claim rule back at me. One gap: on its first run, repro-check
reported it couldn't find my voice guide even though the symlink worked,
so the guard was the only check on my claim against my rules; the second
run found it. My
`CLAUDE.local.md` records the one setup trap I hit: `go vet` in
`studio/` fails until the frontend is built, because `main.go` embeds
`frontend/dist`.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue through the Issue Selection site. It's a pretty
straightforward and simple change, which lets me learn the entire flow
of contributing to open source (claim, reproduce, plan, PR, review)
without the code being the hard part. I wanted to get comfortable first
before moving on to more difficult issues. I also hope to learn how a
real project's checks, sign-off rules, and changelog fit together, and
to get more comfortable reading Go I didn't write.

My scout's scan graded five issues and accepted two, but I passed on
both: code-charity/youtube#2600 is a 2024 feature request that no
maintainer has responded to (the repo is active, but nobody had signed
off on that feature, and a commenter linked a near-duplicate), and
csesnitw/internito#39 was a one-line footer text change.
I graded this issue and a second site pick
(philaconvalley/website#130, a fullscreen button) with the scout. Both
were accepted; after the unit 6 scope patch, the difficulty notes came
back straightforward for this one and moderate for #130. The scout
ranked #130 higher on fit because it's TypeScript and this issue is
mostly Go, but I chose this one anyway for the reasons above.

I would say the scout is definitely useful, but it has limitations that
can be improved. Its rubric checks whether the repo is active, not
whether a maintainer has ever engaged with the issue itself, which is
how youtube#2600 passed. Its search also surfaced a lot of issues that
already had discussion and fixes in progress: three of the five it
graded were already claimed in a comment or an open PR, which my
filters couldn't see. The rubric caught all three, and its grading held
up well overall: on this issue it caught that one of the three items
had already been fixed upstream (#477), which I confirmed in the file's
history and put in my claim.

**Claim comment:** https://github.com/AlphaWaveSystems/flutter-probe/issues/401#issuecomment-6536807923

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran one scan,
  then graded my two Issue Selection site picks directly, and re-graded
  both after the scope patch.
- What it produced: a scan of 387 ranked results with five graded
  (two accepts, three rejects for "already claimed"); an accept for
  this issue on all 8 required checks with a "straightforward"
  difficulty note, plus the finding that the `go.mod` item was already
  done.
- Difficulties: three of five scan slots went to issues claimed
  through comments or linked PRs, and one accept was a feature request
  no maintainer had ever responded to.
- What changed: added `-linked:pr` to every query and moved the stale
  `--updated` date forward; replaced my "Bounded scope" check with the
  scope-feasible row and added the difficulty note. I'd still add a
  check for whether a maintainer has ever engaged with the issue.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

macOS 27.0.1 (Apple Silicon), Go 1.27.3 (the repo requires 1.26.10 or
newer), Node v26.0.0, npm 11.12.1. I forked the repo to
pkmaster21/flutter-probe, cloned my fork, and worked from `main` at
26716d5. I didn't have Go, so I installed it with Homebrew.

One challenge: my first `go vet ./...` in `studio/` failed with
`main.go:13:12: pattern all:frontend/dist: no matching files found`.
`studio/main.go` embeds the built frontend, so the frontend has to be
built first. Running `npm ci && npm run build` in `studio/frontend`
before `go vet` fixed it.

**Steps to Reproduce**

1. Search the repo for callers of `discoveryContext`:
   `grep -rn "discoveryContext" . --exclude-dir=node_modules --exclude-dir=.git`.
   Only match: its own definition at `studio/wifi_discovery.go:127`.
2. Search the repo for anything importing `app.css`:
   `grep -rn 'app\.css' . --exclude-dir=node_modules --exclude-dir=.git`.
   No matches; `studio/frontend/src/main.ts` imports only `./style.css`.
3. Look for the commented-out `replace` in `studio/go.mod`:
   `grep -n replace studio/go.mod`. Only the live
   `replace github.com/alphawavesystems/flutter-probe => ..` on line 8.
4. Run the issue's acceptance checks on unchanged `main`:
   `npm run build` in `studio/frontend` (exit 0, chunk-size warning
   only), then `go vet ./...` in `studio/` (exit 0).

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** grep and build results above;
  [e958c3d](https://github.com/AlphaWaveSystems/flutter-probe/commit/e958c3dfb9e779c3b60717337c68986dd5e33d5d)
  (#477) is the upstream commit that removed the `go.mod` line.
- **My findings:** 2 of the 3 leftovers are still on `main`:
  `discoveryContext` has no callers and `app.css` is imported by
  nothing (it's leftover Wails template CSS like `#logo` and
  `.input-box`). The third, the commented-out `wails/v2` `replace` line
  pointing at a local module cache, was removed in #477 on 2026-10-25,
  after the issue was opened, so my change covers only the first two.
  Both acceptance checks already pass, so a failure after my change
  would come from my change. `go vet` doesn't report unused functions,
  so grep is the real check that `discoveryContext` is gone.

**Toolkit**

- Tool: repro-check, which graded my claim draft alone and then my
  claim plus repro report together before I moved the report here.
- What it produced: accept on both runs; on the second it verified
  each finding against upstream `main`, including that e958c3d removed
  the `go.mod` line.
- Difficulties: it reported that it couldn't find my voice guide (the
  file is a symlink), so its voice notes didn't come from my rules
  until the second run. It also flagged that I summarized grep output
  instead of pasting it.
- What changed: I relied on the voice guard's direct check for my
  rules, and I'll paste raw command output in `test_evidence.md` for
  the PR.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
