# Evidence guide: where proof lives in a reproduction package

For each proof family the rubric checks, this guide says where the evidence
lives and what good looks like. It is the single home for the edge cases and
examples behind each check; `rubric.md` holds only the pass/fail rule and
points back to the section here.

## Environment

**Where it lives:** the repro report's environment record (tool/runtime
version, OS, and, where the repo publishes one, a commit or build
hash). In an eval bundle, this is usually a labelled "Environment"
line near the top of the report. In live mode, it's the equivalent
lines in the student's draft, read against the issue's own stated
version/OS in the issue body or template fields.

**What good looks like:** version and OS are both named, specifically
enough that a stranger could get the same build without guessing
(e.g. "hyperfine 1.20.0 · macOS 26.5", not "latest, on my Mac"). Where
the repo publishes commit/build hashes, one is included. If the tested
build differs from the issue's own stated environment, the delta is
named explicitly ("issue filed against 4.53.2; I tested 4.53.3"). A
silent difference does not pass even if both values are individually
well-formed.

## Steps

**Where it lives:** the repro report's ordered steps section
(preparation/execution, or the repo's own clone → checkout → build →
run shape). In live mode, the draft's own step list, read against the
target repo's README or CONTRIBUTING docs for the real build/run
sequence.

**What good looks like:** an unbroken, ordered sequence starting from
whatever clean state actually applies: a source build (clone →
checkout → build → run) only when the issue requires building from
source; for an already-released CLI/library, the installed
tool-plus-version and a command is a legitimate starting point on its
own, with no build step required. Whichever shape applies, no step
that affects the trigger is skipped, assumed, or left implicit: a
stranger following exactly what's written, and nothing else, reaches
the same command and hits the same trigger the report ran. A step
that silently substitutes a different flag, input, or syntax than the
one being reproduced (even if the substitution looks equivalent)
breaks this, because it changes what gets triggered.

A detail that does not affect the trigger can stay descriptive rather
than literal: "a minimal `env.yml` with a valid `dependencies:` list
plus a `category:` section" is fine when the bug fires on the
`category:` section alone and the dependency list's actual contents
are irrelevant to reproducing it. Don't fail a report for not pasting
a file verbatim when nothing about that file's specific contents
matters to hitting the bug; only fail when the missing specific is one
a stranger would actually need to reach the same trigger.

## Behavior shown

**Where it lives:** the report's pasted output, log excerpt, or
screenshot (the "execution"/"actual" block), read directly against
the issue's own quoted error text, stack trace, exit code, or panic
signature, not against the issue's title or summary alone.

**What good looks like:** read this against what the report itself
claims. If it claims to reproduce, the artifact must show the same
failure class as the one the issue reports, same error message or
panic, same crash site, same exit code, not a different failure
produced by a different input or code path. A syntax error or
input-validation message is not the same failure as a runtime panic,
even when both are "failures" and even when the write-up narrates one
as confirming the other. If the artifact is missing entirely, this
fails regardless of what the surrounding prose claims.

If instead the report honestly claims it could *not* reproduce, this
family is satisfied by a real attempted run being shown, even though
that run's output is normal, non-failing behavior. That absence of
the failure is exactly what a faithful cannot-reproduce looks like,
not a sign the evidence is weak. Don't fail a cannot-reproduce for not
showing the issue's failure; that's the point of the attempt.

## Honesty

**Where it lives:** the report's explicit expected/actual (or
equivalent) lines, and its closing narration: the sentence(s) that
state the conclusion ("this confirms...", "I could not reproduce...",
"partially reproduces...").

**What good looks like:** expected and actual are each specific enough
to name the delta ("expected: emits YAML; actual: `panic: not a
string` at `convertHclExprToNode`", not "it failed"). Separately, the
closing conclusion claims no more than what Behavior shown actually
demonstrates: an honest, evidenced "I could not reproduce this" is a
pass; a confident "this confirms the bug" resting on an artifact that
doesn't match the issue's failure (see Behavior shown) is not, no
matter how much surrounding detail (run counts, extra environments
tried) the report adds around it.

## Comms

**Where it lives:** the repo-facts block's contribution policy
(CONTRIBUTING.md language or issue-template fields on AI-assistance
disclosure), read against the actual text of the claim comment and
repro comment, in an eval bundle both are given; in live mode, the
student's draft and the issue thread's own posted comments.

**What good looks like:** if the policy requires disclosing AI
assistance, the comment(s) state it plainly; if the policy is silent
or only cautions ("review AI output before submitting"), no disclosure
is required to pass. This is a straight read of stated policy against
stated text: don't infer a disclosure requirement from a policy that
doesn't say so, and don't accept an implied disclosure ("I used some
tools to help") where the policy asks for an explicit one.

Separately, the claim comment itself is read against the issue, not
just the repo: **where it lives** is the candidate claim comment's own
text. **What good looks like:** it names something specific to this
issue, a file, a symptom, a detail from the thread, not phrasing
interchangeable across any issue in any repo ("great project, assign
it to me!"), and it promises investigation only, never a guaranteed
fix or a delivery date. An otherwise-solid repro report does not buy
this back; a boilerplate, over-promising claim is its own failure,
independent of how good the report attached to it is.
