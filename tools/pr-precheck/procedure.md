# Procedure: how this tool grades a PR package

## Read order

Read in this order, and write down the notes each step asks for
before moving on. Nothing is graded until step 7 is done.

1. **Repo facts first** (live: `.github/PULL_REQUEST_TEMPLATE.md`,
   `docs/CONTRIBUTING.md`, and the course rule that every PR discloses
   AI use). Write down, as a numbered list, every ask the repo makes
   of a PR: each template section, each checklist item, the issue-link
   form, any required changelog, whatsnew, or docs entry, the title
   convention, the checks that must pass, and whether AI-use
   disclosure is required. If there is no template and no asks, write
   "no stated asks". This list is what template-and-asks-met and
   policy-and-thread are graded against.
2. **Issue and thread second** (live: the issue page and its comments).
   Write the reported bad behavior in one sentence. Then list every
   comment from a maintainer, owner, member, or collaborator that gives
   direction: a requested approach, a constraint, a question to the
   implementer. If there are none, write "no maintainer direction".
3. **Plan third** (live: `plan.md`), all of it, including the repro
   evidence it built on and the `## Deviations` section. Write down
   four lists: (a) planned changes, one line each, with the file or
   area named; (b) the not-in-scope items; (c) the test plan's failing
   repros or failure modes, each with its expected-after result, plus
   any controls and any named checks or suites; (d) every deviation or
   deferral note. Read the plan before the diff so the description's
   confident wording cannot set your expectations about what changed.
4. **Diff fourth** (live: `git diff main...HEAD`). For each file, and
   each hunk inside it, write one line: what the hunk does, in your own
   words, from the code itself.
5. **Commits fifth** (live: `git log --oneline main..HEAD` and
   `git branch --show-current`). Copy the commit messages.
6. **Test evidence sixth** (live: `test_evidence.md`). Write down each
   command or step shown, and its visible output or result.
7. **Title and description last** (live: `pr_draft.md`, first line is
   the title). Copy out every sentence that claims what the PR changes,
   what it tested, or what it leaves out.

The description is read last on purpose: it is the author's account of
the diff, and every side-by-side check below compares that account to
what the diff and the evidence actually show.

## Evidence gathering

Use `references/evidence-guide.md` for where each family lives. For
each check, build the pairing below and keep quotes, not paraphrases.

1. **diff-matches-plan** (plan fidelity). Make a two-column table.
   Left: each hunk from read-order step 4. Right: the planned change
   or deviation note it implements, or "test for the fix", or
   "required entry (repo ask #N)", or "NOT IN PLAN". Then go the other
   way: for each planned change from step 3(a), write the hunk that
   delivers it, or "MISSING" plus the note that discloses it, or
   "MISSING, no note". Mark any hunk that touches something in the
   not-in-scope list.
2. **description-matches-diff** (plan fidelity). For each claim
   sentence from step 7, write the hunk or file that makes it true, or
   "contradicted by <hunk>", or "not in diff". Pay special attention to
   claims of fidelity ("exactly", "no changes beyond", "only") and
   claims that something was added (docs, tests, entries).
3. **repro-before-after** (test evidence). Sort the test plan's cases
   from step 3(c) into two groups. Failing cases: the bug the issue
   reports, in each failure mode the repro evidence or plan names. For
   each, write the before-result shown (or "none"), the after-result
   shown (or "none"), whether the after matches the plan's
   expected-after, and whether the input exercised is the failing one
   or only a control or an unchanged path (compare the inputs used
   with the repro's inputs). Other cases: controls and extra cases for
   new behavior the fix adds. For each, quote the sentence reporting
   its result, or write "silent".
4. **repo-checks-run** (test evidence). List every test suite or
   command named in step 3(c) and every command the repo asks from
   step 1 say must pass. Next to each, quote the outcome (pasted
   output or a sentence naming the command and result), or write
   "not shown". For a failed check, quote the reason given. Leave
   non-command checklist items (code formatted, i18n, docs) to
   template-and-asks-met.
5. **diff-is-clean** (diff quality). Scan every added and removed line
   from step 4 and list each one that is: debug output, commented-out
   code, dead code, a new TODO/FIXME, re-formatting or re-ordering of
   lines the fix does not need, or an unrelated hunk. An explanatory
   comment about the fix is not debris. An import added because the
   fix uses it is not debris.
6. **template-and-asks-met** (standards). For each ask from step 1,
   quote where the title, description, or diff satisfies it, or write
   "missing". For each ticked checkbox in the description, quote the
   evidence that backs it, or write "unsupported".
7. **policy-and-thread** (standards). If disclosure is required, quote
   the disclosure sentence, or write "missing". For each maintainer
   direction from step 2, quote the hunk or description line that
   follows or engages it, or write "ignored".
8. **commits-and-branch-tidy**. Use the commit messages and branch
   name from step 5 as they are.

## Check execution

1. Run the checks in rubric order: diff-matches-plan,
   description-matches-diff, repro-before-after, repo-checks-run,
   diff-is-clean, template-and-asks-met, policy-and-thread, then
   commits-and-branch-tidy. Grade every check, even after one has
   failed: the student needs the full list.
2. Grade each check only from its gathered evidence and the pass
   condition in `rubric.md`. Do not re-read the whole package unless
   the gathered evidence for that check is empty.
3. If the gathered evidence is empty, search the whole package once.
   If it is still absent, grade `fail` when the pass condition needs
   something the PR must show (a before/after, a check's outcome, a
   template section, a disclosure). Grade `unclear` only when the PR
   shows something but the package lacks what you would need to judge
   it (for example, the plan names no expected-after result to compare
   against). Grade `pass` by default only where the rubric says so (no
   stated asks, no policy, no maintainer direction, no named checks).
4. Apply deviation notes before failing anything. A gap that a
   deviation note in the plan, or a plain statement in the description,
   discloses and explains is part of the scope being graded, not a
   failure.
5. Grade the substance, not the format. A short description can pass
   every check; a long, polished one can fail any. Never accept the
   description's account of the diff without the pairing from
   evidence gathering step 1 and 2.
6. For each check, write one line of evidence: the quote or fact that
   decided it. For a fail, name the specific hunk, missing item, or
   claim (for example "diff adds `resolve_symlinks` config option; plan
   scopes out config changes").

## Verdict assembly

1. Apply the verdict rule in `rubric.md`: accept only if every
   required check is `pass`.
2. Any required check graded `fail` or `unclear` makes the verdict
   reject.
3. The preferred check is reported but never changes the verdict.
4. Name the deciding check in the summary. For reject, it is the
   first required check in rubric order that did not pass, with its
   evidence quoted. For accept, write "all required checks pass".
5. In live mode, after the per-check lines, list what to fix, in
   rubric order, and the voice-guide notes.
6. Emit the JSON block from SKILL.md last, with one entry per rubric
   check in rubric order, and nothing after it.
