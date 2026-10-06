# Procedure: how this skill grades a plan package

## Read order

1. Read the Issue section first. Write down, in one sentence, the
   behavior the reporter says is wrong and what they expected. This is
   the boundary every later scope decision is measured against.
2. Read the Repro evidence section second, before the plan. For each
   numbered step, write down what was run and what was observed. Mark
   every control run separately: a step that changes one condition
   (a flag removed, a component swapped out, a setting turned off, the
   same input in a different context) and records whether the bug
   still appears. Write down what each control rules in or rules out.
   Read the evidence before the plan so the plan's confident wording
   cannot set your expectations about the cause.
3. Read the Thread highlights third. Write down every comment from a
   maintainer, owner, or member that gives direction: a named culprit,
   a preferred or rejected approach, a request (test this, don't do
   that), or a decision the thread settled. If there are none, write
   "no maintainer direction".
4. Read the Repo facts block fourth. Write down the contribution
   policy, especially any AI-use disclosure requirement and any
   stated contribution limits. If none is stated, write "no stated
   requirement".
5. Read the Candidate plan fifth, all the way through, before grading.
   Identify these parts wherever they appear (headings are not
   required): the stated cause, the list of changes and the files they
   touch, the not-in-scope statement, the test plan, and any risks or
   unknowns.
6. Read the Candidate plan comment last.

In live mode, the Issue and Thread highlights come from the issue
page on GitHub, the Repo facts come from the repo's README,
CONTRIBUTING, and any AI policy file, and the Repro evidence is the
student's posted repro comment as quoted in plan.md. The candidate
plan is plan.md and the candidate comment is comment.md.

## Evidence gathering

Use `references/evidence-guide.md` for where each family lives. For
each check, collect a quote, not a paraphrase:

1. **diagnosis-matches-evidence**: quote the plan's stated cause in
   one line. Then list each repro step and control from your read-order
   notes next to it, and for each one write "consistent",
   "contradicts", or "not related". A control contradicts the cause if
   it shows the bug with the blamed component absent or unchanged, or
   shows no bug with the blamed component present. Also note whether
   the plan's cause simply repeats a claim from the thread without
   pointing at a repro step.
2. **within-issue-scope**: list every change the plan proposes, one
   line each, with the file or area it names. Quote the not-in-scope
   statement. Next to each change, write whether it is needed to make
   the reproduced behavior correct ("needed") or is extra work
   ("extra": a refactor, migration, upgrade, new option or setting, UI
   change, retry framework, module restructure, or rewrite of a larger
   component). Do not require the named files to appear in the repro
   evidence.
3. **test-targets-cause**: quote the test plan. Write down the exact
   observable result it expects after the fix (an output, exit code,
   value, color, or visible behavior). If it names none, write "no
   observable". Then write whether that result would still be wrong if
   the stated cause were still present.
4. **stranger-could-start**: quote the plan's approach. Write down
   whether it picks one approach and names where the edit goes. Copy
   out every hedge word or deferred decision you find ("somewhere",
   "investigate", "profile", "not sure", "whichever is easier", "maybe
   also", a list of candidate layers with no choice).
5. **comment-follows-thread-and-policy**: from your read-order notes,
   take the maintainer direction and the policy requirements. For each
   one, quote the line in the Candidate plan comment that engages or
   satisfies it, or write "missing".
6. **unknowns-named**: quote any risk or unknown the plan states, and
   how it will be checked.

## Check execution

1. Run the checks in rubric order: diagnosis-matches-evidence first,
   then within-issue-scope, test-targets-cause, stranger-could-start,
   comment-follows-thread-and-policy, and unknowns-named last.
   test-targets-cause uses the cause recorded for the first check.
2. Grade each check only against its gathered evidence and the pass
   condition in `rubric.md`. Do not re-read the whole package for a
   check unless the evidence you gathered is empty.
3. If the evidence is empty, search the whole package once for it.
   If it is still not there, grade `fail` when the rubric's pass
   condition needs something the plan must state (a cause, a file, a
   test outcome, a chosen approach). Grade `unclear` only when the
   plan states something but the package lacks the information needed
   to judge it (for example, the repro evidence has no step that bears
   on the stated cause).
4. Grade the substance, not the format. A short plan with no headings
   can pass every check; a long, confident plan can fail any of them.
   Do not let length, polish, or confident tone change a grade.
5. For each check, write one line of evidence: the quote or fact that
   decided the grade. For a fail, quote the line that failed it (the
   contradicting control step, the extra change, the vague test, the
   hedge, the ignored maintainer comment, or "no AI-use disclosure
   while policy requires it").

## Verdict assembly

1. Apply the verdict rule in `rubric.md`: accept only if every
   required check is `pass`.
2. Any required check graded `fail` or `unclear` makes the verdict
   reject.
3. Preferred checks are listed in the output but never change the
   verdict.
4. In the summary, name the deciding check: for reject, the first
   required check that did not pass, with its quoted evidence; for
   accept, write "all required checks pass".
5. Emit the JSON block from SKILL.md last, with one entry per rubric
   check in rubric order.
