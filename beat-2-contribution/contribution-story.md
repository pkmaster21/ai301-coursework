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
**Pull Request:** https://github.com/AlphaWaveSystems/flutter-probe/pull/519

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

**Unit 7: Wrote a ship manifest and got my PR through the ship-gate**

My manifest started with four checks from the issue, the PR template,
and the Studio CI file, and all four passed on `main` before I changed
anything. On my branch, the gate ran them fine, but my pr-precheck
rejected the first run: CONTRIBUTING.md also asks for `make test` and
`flutter test`, and it couldn't see my DCO sign-off. I added `make test`
to the manifest, moved `flutter test` to the excluded table with a dated
reason (my PR touches no Dart), and put the evidence in the PR, and it
passed; my voice guard then caught two lines in the description that
claimed more than my pasted evidence showed or left out that I'd
reviewed the code.

**Unit 8: Installed the PR monitor and wrote a playbook for flutter-probe**

The monitor read PR #519 six days after I opened it: one passing check,
no reviews, and two comments, the CLA bot's and my own CLA signature.
It drafted a wait note, not a reply, and I sent nothing, because no
maintainer has said anything yet and my 7-day first-response threshold
hadn't passed. What fell short: `--state` labeled the PR FEEDBACK-IN
both times I ran it, because the monitor doesn't filter out bots and
the CLA bot's comment came after my last commit. The draft only came
out right because my playbook already says a bot comment is not review
feedback, so I'm keeping that rule and I check the run report's state
against the actual thread instead of trusting the label.

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

Nothing is broken for users. `studio/`, the FlutterProbe desktop app,
carries leftovers that make the code harder to read: a Go helper
function nothing calls, a CSS file nothing imports, and (when the issue
was filed) a commented-out `go.mod` line pointing at one developer's
local module cache.

**Expected Behavior**

All three leftovers are gone, a search finds no remaining references,
and `go vet ./...` in `studio/` and `npm run build` in
`studio/frontend` still pass.

**Current Behavior**

On `main`, `discoveryContext` is still defined in
`studio/wifi_discovery.go` with no callers, and
`studio/frontend/src/app.css` still exists with nothing importing it.
The `go.mod` line was already removed upstream in #477. Both checks
pass today.

**Affected Components**

- `studio/wifi_discovery.go` (the `discoveryContext` function and the
  `time` import only it uses)
- `studio/frontend/src/app.css`
- `CHANGELOG.md` at the repo root (the PR template requires an entry)

**Toolkit**

- Tool: Claude Code with my `CLAUDE.local.md`, used for one scoped
  read of `studio/` (the files the issue names, `main.go`, and
  `main.ts`) instead of the whole repo.
- What it produced: the short list of files above, plus two facts the
  issue doesn't mention: `discoveryContext` is the only use of the
  `time` import, and `main.go` embeds the built frontend.
- Difficulties: `go vet` doesn't flag unused functions, so it can't
  confirm the dead code is dead; grep had to.
- What changed: I added the `time` import to the plan so removing the
  function doesn't break the build.

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

Both remaining leftovers were dead from the commit that added them.
`discoveryContext` in `studio/wifi_discovery.go` arrived with the mDNS
WiFi discovery feature (#86, 2026-05-02), but `wifiDiscovery.Start()`
in that same commit builds its context directly with
`context.WithCancel(context.Background())`, so the helper never had a
caller. It is also the only code in the file that uses the `time`
package. `studio/frontend/src/app.css` arrived with the Studio beta
(#77, 2026-04-26) and holds Wails starter-template styles (`#logo`,
`.result`, `.input-box`); `main.ts` has only ever imported
`./style.css`. The third item, the commented-out `wails/v2` `replace`
in `studio/go.mod`, was already removed in #477.

**Proposed Solution**

Delete the dead function and the now-unused `time` import, delete
`app.css`, and add one line to the root `CHANGELOG.md` under
`[Unreleased]`. I'm deliberately not changing the live
`replace github.com/alphawavesystems/flutter-probe => ..` in
`studio/go.mod` (it's in use and documented), `style.css`, or anything
else in `wifi_discovery.go`. No tests are added: deleting uncalled code
changes no behavior, so the evidence is a search showing no references
plus the existing checks passing.

**Implementation Plan**

1. Branch from `main`: `fix/401-studio-dead-code`. (Deviation: I named the branch `fix/studio-dead-code` instead; CONTRIBUTING only asks for a descriptive name.)
2. In `studio/wifi_discovery.go`, delete `discoveryContext` and its
   comment, and remove `"time"` from the imports. Run `gofmt`.
3. Delete `studio/frontend/src/app.css`.
4. Add a `### Removed` entry under `## [Unreleased]` in `CHANGELOG.md`.
5. Re-run the searches, limited so my own CHANGELOG line can't match:
   `git grep -n discoveryContext -- studio/` and `git grep -n "app\.css" -- studio/`
   should both print nothing; then run the manifest checks: `npm ci && npm run build`, then
   `go vet ./...`, `go build ./...`, `go test ./...` in `studio/`.
6. Commit with `git commit -s` (DCO sign-off).

Deviation (2026-10-30): the plan only limited the `app.css` search to
`studio/`. On the branch, a repo-wide `git grep discoveryContext`
matched my own CHANGELOG line, the same trap plan-check caught for
`app.css`, so I limited that search to `studio/` too. I also added
`ls studio/frontend/src/app.css` before and after, because the `app.css`
reference search prints nothing on `main` either and so proves nothing
on its own.


**Baseline evidence (on `main` at 26716d5, 2026-10-30)**

```
$ git grep -n discoveryContext
studio/wifi_discovery.go:127:func discoveryContext(parent context.Context) (context.Context, context.CancelFunc) {
$ git grep -n "app\.css" -- studio/
(no output, exit 1)
$ cd studio/frontend && npm run build
✓ built in 2.63s   (exit 0; only warning: "Some chunks are larger than 500 kB after minification")
$ cd studio && go vet ./...
(no output, exit 0)
```

**Risks and unknowns**

- A repo-wide search for `app.css` would match my own CHANGELOG entry,
  so the after-check searches `studio/` only.
- `go vet`, `go build`, and `go test` in `studio/` fail until the
  frontend is built (`main.go` embeds `frontend/dist`), so the
  frontend build runs first in every check.
- `go vet` doesn't report unused functions, so it can't prove
  `discoveryContext` is gone; the `git grep` is that check.
- The maintainer may prefer a different CHANGELOG section for a
  cleanup; I'll use `### Removed` and adjust if asked.

**Toolkit**

- Tool: plan-check, grading `plan.md` and a draft plan comment
  together (the comment stays unposted; my PR carries it).
- What it produced: reject (hold) on the first run, because neither
  draft showed any baseline output and the plan named no risks; accept
  on the second.
- Difficulties: it caught a real bug in my test plan: a repo-wide
  search for `app.css` would have matched my own CHANGELOG line. It
  also couldn't find my voice guide (the symlink again), expected the
  comment in `comment.md`, and has no idea what "repro evidence" means
  for a cleanup issue with no bug.
- What changed: I pasted the real baseline output into the plan,
  added a risks section, and limited the `app.css` search to `studio/`.

---

## Testing Strategy

The change only deletes code nothing uses, so there's no new behavior
to test. Instead, three things have to be observable:

1. **The leftovers are gone.** Before the fix, on `main` at 26716d5,
   `git grep -n discoveryContext -- studio/` prints the definition at
   `studio/wifi_discovery.go:127` and `ls studio/frontend/src/app.css`
   finds the file. After the fix, on `fix/studio-dead-code`, the
   grep prints nothing (exit 1) and `ls` reports "No such file or
   directory". Both searches are limited to `studio/` because my
   CHANGELOG line names both items.
2. **Nothing referenced them.** `git grep -n "app\.css" -- studio/`
   prints nothing before or after, and the build below compiles without
   the deleted function or file.
3. **The project's own checks still pass**, the same four from my ship
   manifest that passed on `main` before I changed anything:
   `npm ci && npm run build` in `studio/frontend` (exit 0, only the
   existing chunk-size warning), then `go vet ./...` (exit 0),
   `go build ./...` (exit 0), and `go test ./...` in `studio/`
   (`ok .../studio 10.573s`, `ok .../studio/e2e`). `gofmt -l` lists no
   files.

No unit test was added: there's no behavior to pin, and a test that a
function doesn't exist would be the compiler's job. The full output is
in my PR description.

**Toolkit**

- Tool: my ship manifest's four checks, run by hand on `main` and
  then on my branch, plus the before and after searches from my
  reproduction.
- What it produced: identical passing checks before and after, and
  searches that find both leftovers on `main` and neither on my branch.
- Difficulties: my first after-run looked like a failure:
  `git grep discoveryContext` matched my own CHANGELOG line. And my
  "before" `app.css` search printed nothing, which proved nothing,
  since the file was unreferenced before the fix too.
- What changed: I limited both searches to `studio/` and added an
  `ls` of `app.css` before and after, so each piece of evidence can
  actually tell the two states apart.

---

## Implementation Notes

**2026-10-30**

Forked and cloned the repo, installed Go with Homebrew, and hit the
build order trap: `go vet` in `studio/` fails until the frontend is
built, because `main.go` embeds `frontend/dist`. Confirmed on `main`
that two of the three leftovers are still there and the `go.mod` one
was removed in #477. Baseline: all four checks pass.

**2026-10-30**

Traced both leftovers with `git log`: `discoveryContext` came in with
#86 and was never called (`Start()` builds its own context), and
`app.css` came in with #77 and was never imported. Wrote the plan;
plan-check held it until I pasted real baseline output and named
risks, and it caught that a repo-wide `app.css` search would match my
CHANGELOG line.

**2026-10-30**

Built the fix on `fix/401-studio-dead-code`: remove `discoveryContext`
and the `time` import only it used, delete `app.css`, and add a
CHANGELOG entry. Claude Code first made these edits and committed
them as three separate commits without asking me. I had it reset, then
started a fresh branch from `main`, made the deletions myself from
what the issue asks for, kept Claude's drafted CHANGELOG entry because
it still matched my change, and made one signed-off commit
(`git commit -s`, per the DCO rule), since the whole fix is one small
cleanup. I then amended it to sign off as David Z with my GitHub
noreply address, to keep my personal email out of public history. For the CHANGELOG I used a
`### Removed` heading: the repo says it follows Keep a Changelog, which
defines that section, but it has never actually used it, so I flagged
that as a risk.

**2026-10-30**

On the branch, a repo-wide `git grep discoveryContext` matched my own
CHANGELOG line, the same trap as `app.css`. Limited it to `studio/`,
added an `ls` of `app.css` before and after, and recorded the change
as a deviation under Implementation Plan. All four checks pass on the
branch.

**2026-11-06**

Ran the PR monitor on `fix/studio-dead-code`. No maintainer activity
yet, checks still green, and the branch is now behind `main` but still
mergeable with no conflicts. I left the branch alone for now: a rebase
isn't needed to merge, and if I rebase later I'll re-run every manifest
check before force-pushing.

**Toolkit**

- Tool: Claude Code in my clone, with `CLAUDE.local.md` loaded, for
  the edits, commits, and evidence runs.
- What it produced: a first version of the fix (which I discarded,
  then made the deletions myself from the issue), a CHANGELOG entry I
  kept because it still matched my change, and `test_evidence.md`.
- Difficulties: it went ahead and built and committed the fix as
  three commits when I wanted to make the change myself, and the
  evidence searches tripped over my own CHANGELOG line.
- What changed: I reset to a fresh branch, made the deletions and one
  commit myself, and scoped the searches to `studio/`.

---

## Pull Request

**PR Link:** https://github.com/AlphaWaveSystems/flutter-probe/pull/519

**PR Description:** "chore(studio): remove unused discoveryContext and
app.css", opened 2026-10-30. It says `Fixes #401`, explains that
`discoveryContext` (added in #86) was never called and `app.css` (added
in #77) was never imported, and notes the `go.mod` item was already
handled in #477. It shows before and after searches, the checks passing
on my branch (`npm run build`, `go vet`, `go build`, `go test`,
`make test`), why `flutter test` wasn't run, an AI use section, and the
repo's full PR template checklist with N/A reasons for the sections
that don't apply. After opening it, I signed the project's CLA with a
comment on the PR, and the `cla` check passed.

**Maintainer Feedback:**

- 2026-11-06: none yet. Six days after opening, the only comments are
  the CLA bot's and my CLA signature; no review has been requested
  back or submitted.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate, which ran my manifest's checks and then my
  pr-precheck on my branch, plan, evidence, and description, plus my
  voice guard on the description.
- What it produced: a reject on the first run (`make test` and
  `flutter test` missing from my evidence, DCO sign-off unclear), then
  PASS after I fixed those, with notes I also acted on: a line claiming
  every check had passed on `main` when I'd only shown two, and a stale
  branch name in my evidence and plan.
- Difficulties: pr-precheck only read one-line commit titles, so it
  couldn't see my sign-off until I pasted it, and it can't run
  `flutter test` (no Flutter SDK). My voice guard stopped the
  description twice: once for a "checks pass" line with no output
  shown, and once for an AI disclosure without "I reviewed every line".
- What changed: added `make test` to my manifest and moved
  `flutter test` to the excluded table with a dated reason, fixed both
  description lines, and recorded the branch rename as a plan
  deviation.

---

## Learnings and Reflections

**Technical Skills Gained**

- Reading Go I didn't write: following a function's history with
  `git log` to show it never had a caller, and learning that `go vet`
  doesn't report unused functions, so `git grep` had to be the proof.
- How Go's `embed` affects build order: `studio/main.go` embeds
  `frontend/dist`, so `go vet`, `go build`, and `go test` all fail
  until the frontend is built.
- Writing evidence that tells two states apart: scoping searches to
  `studio/` so my own CHANGELOG line can't match, and adding an `ls`
  before and after because an empty search on `main` proved nothing.
- A real project's contribution rules: DCO sign-off with
  `git commit -s`, a CLA signed by comment, the PR template's
  checklist, and a Keep a Changelog entry.
- Putting the tools I built in class to work on a real repo, one per
  phase: issue-scout to pick the issue, repro-check on my claim and
  repro report, plan-check on my plan, pr-precheck and my ship manifest
  (through the ship-gate) before opening the PR, my voice guard on
  everything I posted, and the PR monitor afterward. Each one caught
  something I'd missed (a claimed issue, a search that matched my own
  CHANGELOG line, missing `make test` evidence, an overclaiming line),
  and running them on real work showed me where they fall short, like
  the scout not checking maintainer engagement and the monitor counting
  a bot comment as feedback.

**Challenges Overcome**

- The build failed before I changed anything (`pattern
  all:frontend/dist: no matching files found`). Reading `main.go`
  showed the embed, and building the frontend first fixed it.
- Part of the issue was already done: #477 removed the `go.mod` line
  after the issue was filed. I confirmed it in the file's history and
  said so in my claim and PR instead of pretending to fix it.
- Claude Code built and committed the fix as three commits before I
  wanted it to. I reset, started a fresh branch, made the deletions
  myself, and made one signed-off commit.
- My pr-precheck rejected the first PR because my evidence was missing
  `make test` and my sign-off wasn't visible. I added the check to my
  manifest, gave a dated reason for skipping `flutter test`, and pasted
  the evidence.

**What I'd Do Differently Next Time**

- Set my commit name and noreply email before the first commit. I
  amended twice, and the earlier pushed commits still show on the
  issue's timeline.
- Check whether a maintainer has ever engaged with an issue before
  trusting a scout accept, and check the upstream history for each
  item in the issue before claiming it.
- Tell Claude Code up front which steps are mine to do (edits and
  commits), instead of correcting it after it has already acted.

---

## Resources Used

- flutter-probe's CONTRIBUTING.md
  (https://github.com/AlphaWaveSystems/flutter-probe/blob/main/CONTRIBUTING.md):
  the DCO sign-off rule, `make test`, and the branch naming guidance.
- flutter-probe's PR template, which set the CHANGELOG requirement and
  the checklist my description follows.
- Keep a Changelog (https://keepachangelog.com/en/1.1.0/), which the
  repo says it follows, for the `### Removed` section.
- Upstream commit e958c3d (#477)
  (https://github.com/AlphaWaveSystems/flutter-probe/commit/e958c3dfb9e779c3b60717337c68986dd5e33d5d),
  which showed the `go.mod` item was already fixed.
- The commits from #86 and #77, read with `git log`, which showed both
  leftovers were dead from the start.
