# Removing dead code from flutter-probe Studio, and the pre-PR gate that caught what I missed

## Overview

flutter-probe is an open source end-to-end testing tool for Flutter apps, and Studio is its desktop app (Go with a Wails frontend). Issue [#401](https://github.com/AlphaWaveSystems/flutter-probe/issues/401) listed three leftovers in Studio: an uncalled Go helper, an unimported CSS file, and a commented-out `go.mod` line pointing at one developer's local module cache. I found that the `go.mod` line had already been removed upstream in #477, so I removed the other two and opened [PR #519](https://github.com/AlphaWaveSystems/flutter-probe/pull/519). Before opening it, I ran the branch through a ship-gate I built this term: a manifest of the project's own checks that I wrote from its CONTRIBUTING.md, PR template, and CI file, plus a pr-precheck rubric that grades the diff, evidence, and description. The gate rejected my first attempt, I fixed what it found, and the maintainer merged the PR on November 12, 2026, which closed #401.

## What happened

### Checking what was still true

Before claiming the issue, I checked each of its three items against `main`. The `go.mod` line was already gone: commit e958c3d (#477) removed it on October 25, after the issue was filed. I said so in my claim comment instead of claiming all three.

Then I traced the other two with `git log`. `discoveryContext` came in with the mDNS WiFi discovery feature (#86), but `wifiDiscovery.Start()` in that same commit builds its own context with `context.WithCancel`, so the helper never had a caller. `app.css` came in with the Studio beta (#77) and held Wails starter-template styles that `main.ts` never imported.

### A build that failed before I touched anything

My first `go vet ./...` in `studio/` failed on unchanged `main`:

```
main.go:13:12: pattern all:frontend/dist: no matching files found
```

`studio/main.go` embeds the built frontend, so `go vet`, `go build`, and `go test` all fail until `npm run build` has run in `studio/frontend`. Every check I ran afterward builds the frontend first.

### Evidence that can tell two states apart

`go vet` doesn't report unused functions, so a search had to prove the code was gone. My plan-check tool held my first plan and pointed out that a repo-wide search for `app.css` would match my own CHANGELOG line. On the branch, the same thing happened with `discoveryContext`. I limited both searches to `studio/` and added an `ls` of `app.css`, because an empty reference search proves nothing when the file was unreferenced on `main` too. This is the evidence that went into the PR:

```
$ git grep -n discoveryContext -- studio/
before: studio/wifi_discovery.go:127:func discoveryContext(...)
after:  (no output)
$ ls studio/frontend/src/app.css
before: studio/frontend/src/app.css
after:  No such file or directory
```

### The fix

The change is three files: a CHANGELOG entry, the deleted `app.css` (54 lines), and this hunk in `studio/wifi_discovery.go`, including the `time` import that only the helper used:

```diff
 	"sync"
-	"time"
@@ -121,12 +120,3 @@ func entryToDevice(e *zeroconf.ServiceEntry) WiFiDevice {
-// Helper used by App methods to construct a default browse context tied to
-// the App's lifecycle.
-func discoveryContext(parent context.Context) (context.Context, context.CancelFunc) {
-	if parent == nil {
-		parent = context.Background()
-	}
-	return context.WithTimeout(parent, 24*time.Hour) // effectively forever; user stops manually
-}
```

Claude Code first made these edits and committed them as three commits without asking. I reset, started a fresh branch, made the deletions myself, and made one commit with `git commit -s`, since the project requires a DCO sign-off.

### The gate rejected my first attempt

My manifest started with four checks: the frontend build, `go vet`, `go build`, and `go test`. All four passed on my branch, but my pr-precheck rubric still rejected it (gate log, October 30, 22:14):

```
- repo-checks-run [fail]: CONTRIBUTING says run `make test` and `cd probe_agent && flutter test`
  before submitting; neither appears in test_evidence.md or the description.
- template-and-asks-met [unclear]: ... CONTRIBUTING's DCO sign-off requirement cannot be
  verified from the one-line commit log.
```

I had built the manifest from the CI file and missed a step CONTRIBUTING asks for. I added `make test` to the manifest, moved `flutter test` to its excluded table with a dated reason (the PR touches no Dart, and I have no Flutter SDK), and pasted my sign-off into the evidence. The next run passed with five manifest checks:

```
- repo-checks-run [pass]: npm build, go vet, go build, go test (studio), root go build, make test,
  gofmt each shown with exit 0; flutter test not run, with a reason unrelated to the change.
```

My voice guard then stopped the description twice: once for a "tests pass" line my pasted output didn't show, and once for an AI-use note that didn't say I'd reviewed every line. I fixed both and opened the PR on October 30.

### Waiting, then the merge

For twelve days the only comments were the CLA bot's and my own CLA signature. My PR monitor labeled the PR `feedback-in` anyway, because it counted the bot's comment as feedback. My playbook rule that a bot comment isn't review feedback kept it from drafting a reply, so I sent nothing. On November 12, the maintainer merged `main` into my branch, resolved the CHANGELOG conflict ("merge main; CHANGELOG: keep both"), and merged the PR. Issue #401 closed with it.

## Limitations and what's next

The contribution itself is small. It deletes code and changes no behavior, so it never exercised a review of real logic, and the maintainer merged it without comment. My tools fell short in places too. pr-precheck reads only one-line commit titles, which is why it couldn't see my sign-off until I pasted it in. My PR monitor doesn't filter out bot authors, so its state label was wrong on every run, and I had to check the thread myself.

Next, I'll make the monitor ignore bot accounts before it picks a state, and make pr-precheck read full commit messages so a `Signed-off-by` line counts on its own. For my next contribution, I want an issue that changes behavior and needs a new test. That would put the manifest and rubric against a diff where the tests carry the proof, not just searches.
