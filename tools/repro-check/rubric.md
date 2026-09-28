# Rubric: is this reproduction package ready to post?

Each check's pass condition is the decision rule. Where a rule depends
on a judgment call (what counts as a clean starting state, what an
honest cannot-reproduce must show), the detail lives in one place:
the named section of `references/evidence-guide.md`.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Reproducible steps | The repro report's steps, read as a stranger starting from a clean state would read them (evidence guide: Steps) | Pass if a stranger following the steps as written, from the clean state that applies to this issue, reaches the same command and the same trigger the report describes. Fail if a step that affects the trigger is assumed, out of order, or missing. | required |
| Environment pinned | The repro report's environment record, read against the issue's own stated environment (evidence guide: Environment) | Pass if version and OS are both stated, a stranger could obtain the same build without guessing, and any difference from the issue's build is named. Fail if version or OS is missing, or the build silently differs from the issue's. | required |
| Expected vs. actual stated | The repro report's expected/actual lines (or equivalent), read against the issue's own description of the bug (evidence guide: Honesty) | Pass if the report states what should have happened and what did happen, each specific enough to identify the delta. Fail if either is missing or too generic to name what's wrong. | required |
| Behavior matches the issue | The artifact's error/output content, read against the failure the issue describes and against what the report claims (evidence guide: Behavior shown) | If the report claims to reproduce: pass only if the artifact shows the same failure the issue reports; fail if it shows an adjacent or unrelated failure narrated as a match. If the report honestly states it could not reproduce: pass if a real attempted run is shown. | required |
| Outcome stated honestly | The report's stated conclusion (confirmed / cannot-reproduce / partial), read against what its own evidence shows (evidence guide: Honesty) | Pass if the conclusion claims no more than the evidence supports. Fail if the write-up asserts more than its own artifact demonstrates. | required |
| Conventions/disclosure respected | The repo-facts block's contribution policy, read against the claim and repro comments' text (evidence guide: Comms) | Pass if the policy requires no disclosure, or requires disclosure and the comment(s) include it. Fail if the policy requires disclosure and neither comment discloses AI assistance. | required |
| Claim is specific, not boilerplate | The candidate claim comment's text, read against the issue's specifics and against any promised fix or delivery date (evidence guide: Comms) | Pass if the claim names something specific to this issue that couldn't be pasted unchanged onto a different one, and promises investigation only, no guaranteed fix, no delivery date. Fail if the comment is generic "assign me" boilerplate, or promises a specific fix/timeline before the investigation backing it exists. | required |

## Verdict rule

Accept only if every required check passes. There are no preferred
checks yet; if one is added later, it must never change the verdict.
`unclear` on any required check counts as fail, same as a fail.
