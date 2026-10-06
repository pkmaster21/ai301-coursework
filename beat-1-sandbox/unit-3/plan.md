# Plan: issue #60, FaithfulnessChecker crashes on a `text: None` chunk

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60
Repro this plan builds on: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5863732145

## Diagnosis

`FaithfulnessChecker.check()` builds its context string at
`rag/evaluator/faithfulness_checker.py:38`:

```python
context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
```

`dict.get("text", "")` only returns the `""` default when the `text` key is
missing. When the key is present with the value `None`, `.get()` returns
`None`, and `" ".join(...)` raises because it can only join strings.

Evidence from my repro comment:

> Actual:
> ```
> context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
> TypeError: sequence item 0: expected str instance, NoneType found
> ```
> at `rag/evaluator/faithfulness_checker.py:38`, exact same message and line the issue
> reports.

and

> Also reproduced with the issue's own snippet:
> `FaithfulnessChecker().check('Knows Python.', [{'text': None}])`
> produces the identical `TypeError`.

The control that separates a missing key from a `None` value is the existing
`test_missing_text_key_in_chunk` test, which passes on the same commit with
`[{"content": "Python skills"}]` (no `text` key). My repro comment points to it
as the expected behavior: "the same way `test_missing_text_key_in_chunk`
already handles a chunk with no `text` key at all." I ran that control on
unchanged `2f4e82f` before posting this plan:

```
$ .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_missing_text_key_in_chunk
tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_missing_text_key_in_chunk PASSED [100%]
======================= 1 passed, 21 deselected in 0.18s =======================

$ .venv/bin/python -c "
from rag.evaluator.faithfulness_checker import FaithfulnessChecker
print('score =', FaithfulnessChecker().check('Knows Python.', [{'content': 'Python skills'}]))
"
score = 0.0      (exit 0)
```

So `.join()` works when it gets strings; the fault is the `None` value getting
past the `.get()` default.

## Scope

In scope: one change to the expression on line 38 of
`rag/evaluator/faithfulness_checker.py` so a `None` value becomes `""` before
the join. Also, per CONTRIBUTING's "Working on a seeded bug" section, remove the
`@pytest.mark.xfail(strict=True, reason="issue #60: ...")` marker from
`test_none_context_chunk_text` in `tests/unit/test_faithfulness_checker.py`,
because that test passes once the bug is fixed and a strict xfail would then
fail CI.

Not in scope:
- The scoring logic in `_extract_claims()` and `_is_supported()`.
- The caller in `rag/evaluator/eval_suite.py`, or adding validation of chunk
  shape anywhere upstream of the checker.
- Non-string, non-`None` `text` values (for example an int). The issue and my
  repro are about `None` only.
- The other xfail tests in the same file, which belong to issue #59.
- Any lint or type cleanup elsewhere in the file.

## Files to touch

- `rag/evaluator/faithfulness_checker.py`: line 38, inside `check()`.
- `tests/unit/test_faithfulness_checker.py`: delete the xfail decorator above
  `test_none_context_chunk_text` (lines 225 to 228). The test body stays the
  same.

## Approach

Change line 38 to:

```python
context_text = " ".join([chunk.get("text") or "" for chunk in context_chunks])
```

`chunk.get("text")` returns `None` both for a missing key and for an explicit
`None`, and `or ""` turns either into an empty string. A chunk with real text
is unchanged. An empty-string `text` stays `""`, which is the same as today.
This is the same fallback the missing-key case already gets, now applied to
`None` as well.

Order of work, on branch `fix/60-none-context-chunk-text` in my fork:
1. Edit line 38.
2. Remove the issue #60 xfail marker from `test_none_context_chunk_text`.
3. Run the test plan below.

## Test plan

Re-run my Unit 2 repro steps against the fixed branch:

1. The named test, without `--runxfail` (the marker will be gone):
   ```
   .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -v -m unit -k test_none_context_chunk_text
   ```
   Before (from my repro, with `--runxfail`): `1 failed` with
   `TypeError: sequence item 0: expected str instance, NoneType found` at line 38.
   Expected after: `test_none_context_chunk_text PASSED`, `1 passed`.

2. The issue's snippet, printing the result:
   ```
   .venv/bin/python -c "
   from rag.evaluator.faithfulness_checker import FaithfulnessChecker
   print('score =', FaithfulnessChecker().check('Knows Python.', [{'text': None}]))
   "
   ```
   Before: the `TypeError` above, exit 1. Expected after: `score = 0.0`, exit 0
   (the `None` chunk contributes no text, so the one claim has no support).

3. Control, to show the missing-key path is unchanged: run the same snippet with
   `[{'content': 'Python skills'}]`. Expected before and after: `score = 0.0`,
   exit 0.

4. The whole checker test file:
   ```
   .venv/bin/python -m pytest tests/unit/test_faithfulness_checker.py -m unit
   ```
   Expected after: every test passes except the issue #59 xfails, which stay
   xfailed, and there is no `XPASS(strict)` failure.

If the fix were wrong (for example, if `None` still reached the join), steps 1
and 2 would still show the `TypeError`.

## Risks and unknowns

- `or ""` also turns other falsy values (`0`, `False`, `[]`) into `""`. That is
  harmless for the join, but it hides a malformed chunk instead of raising.
  I'm accepting that because the missing-key case already falls back to `""`
  silently.
- A non-string truthy `text` (such as an int) would still raise in the join. I'm
  not addressing that here. I'll check whether any caller can produce one by
  looking at what `eval_suite.py` passes in as `chunks`, and mention it in the
  PR if it can.
- I haven't checked whether mypy flags the new expression, given the
  `list[dict]` annotation. I'll run the repo's type check on the file before
  committing.

## Deviations

Nothing changed; the plan held. The build is the two edits the plan names
(line 38 of `faithfulness_checker.py` and the issue #60 xfail marker), and
every test plan step gave the result it predicted.

Two of the open unknowns are now settled:
- mypy: `mypy rag/evaluator/faithfulness_checker.py` reports
  `Success: no issues found in 1 source file` with the new expression.
- Non-string `text` from callers: `eval_suite.py` passes its `chunks` straight
  through, and nothing in the repo calls `EvalSuite` yet (issue manifest:
  "no code calls `EvalSuite`"). So no caller can currently send an int, and
  I left that case out of scope as planned.
