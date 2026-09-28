# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | "maintainer first-response sample" under Repo facts, and the author names on the last 5 default-branch commits (live: Owner/Member/Collaborator badges on replies to recently updated issues, Issues tab sorted by recently updated; and the commit list on the repo front page) | At least one maintainer (Owner, Member, or Collaborator) reply anywhere in the sample within the last 60 days (measured from now/capture date, not issue-open date), OR at least one of the last 5 default-branch commits is authored by a non-bot account (username not ending in `[bot]`) within the last 30 days | required |
| Repo in use | "last push to any branch" under Repo facts (live: newest commit date on the repo front page / Branches page) | At least one push within the last 30 days (measured from now/capture date). Judge the repo's overall push activity only — an old issue or a "stale" bot label on this issue does not make the repo itself dead | required |
| Release recency | "latest release" under Repo facts (live: Releases box in the repo's right sidebar) | Latest release within the last 90 days | preferred |
| Not already claimed | "this issue: assignees:" and "linked PRs:" under Repo facts, plus the Comments section (live: Assignees box and Development box in the issue's sidebar, plus the thread) | No assignee listed, no open linked PR, and no unanswered "I'll take this" / "working on this" style claim comment in the thread | required |
| No repeated failed attempts | Comment thread text and linked-PR state under Repo facts, on this issue | Fail if 2 or more closed, unmerged linked PRs, or 2 or more separate claim-then-abandon comment cycles by different contributors — a single past attempt is not a track record. Otherwise pass | required |
| Not a tracking/umbrella issue | Issue body | Fail if the issue body is a list/tracker of many other issues (more than 5 linked issue references, or explicitly labeled "megaissue" / "tracking") rather than one concrete change. Otherwise pass | required |
| Not an unresolved bot-filed issue | Issue body and comment thread text | Fail if opened by a bot/automated account, with no maintainer reply, leaving a core implementation detail explicitly unresolved (e.g. "TBD"). Otherwise pass | required |
| Bounded scope | Issue body | Fail if the issue body describes a codebase-wide, multi-module, or umbrella-scale change with no single bounded target (e.g. "add type annotations across the codebase," "refactor all X modules," "migrate every Y to Z"). Otherwise pass | required |
| Contribution policy allows AI use | "contribution policy" line under Repo facts (live: CONTRIBUTING.md or linked contributing guide) | Fail only if the policy is an explicit outright ban on AI-assisted or AI-generated contributions (e.g. "we do not accept AI-generated code or documentation"). Pass if the policy is silent, cautions about review/testing, or conditionally allows AI use | required |

## Verdict rule

Accept the issue only if every required check passes. A required check
graded `unclear` counts as fail. Release recency (preferred) never
changes the verdict; it exists only to rank among accepted issues.
