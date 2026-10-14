# Plan: Output parser crashes on a top-level JSON array fallback (#69)

## Diagnosis

`parse_review_output` in `rag/generator/output_parser.py` sends whatever
`json.loads` returns straight to `_parse_json_output` (line 41 for the
code-fence path, line 48 for the raw-JSON path). `_parse_json_output`
assumes a dict and calls `data.items()` (line 68). When the LLM returns
a top-level JSON array, `json.loads` produces a `list`, and `.items()`
raises:

```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    from rag.generator.output_parser import parse_review_output; print(parse_review_output("[\"First feedback item\", \"Second feedback item\"]"))
                                                                       ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/stephanie/pathreview-ai301-fa26-s3/rag/generator/output_parser.py", line 48, in parse_review_output
    return _parse_json_output(data)
  File "/Users/stephanie/pathreview-ai301-fa26-s3/rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
                      ^^^^^^^^^^
AttributeError: 'list' object has no attribute 'items'
```


From my unit 2 repro: passing the array `["First feedback item",
"Second feedback item"]` to `parse_review_output` crashes at line 48 →
line 68, shown above.

Checked during planning, against the unmodified `output_parser.py` from
`main` (not part of my unit 2 repro):

- Control, a JSON object instead of an array, parses fine:
```
  [FeedbackSection(section_name='skills', content='Expert', confidence=0.85, suggestions=[])]
```
- The same array inside a ```` ```json ```` fence crashes the same way,
  through the code-fence path at line 41:
```
  File "/tmp/op_main.py", line 41, in parse_review_output
    return _parse_json_output(data)
  File "/tmp/op_main.py", line 68, in _parse_json_output
    for key, value in data.items():
  AttributeError: 'list' object has no attribute 'items'
```

So JSON decoding works, and only non-dict values break, on both paths.

That points at the dict-only assumption in `_parse_json_output`, not at
the JSON extraction: the array is found and decoded correctly, and the
crash comes after.

## Scope

In: guard the two calls in `parse_review_output` so only a dict reaches
`_parse_json_output`. Any other decoded JSON value (a list, or a bare
number or string, which hits the same `.items()` crash) falls through to
the existing plain-text fallback, `_parse_plaintext_output(raw)`.
Remove the `xfail` marker from `test_json_array_fallback`.

Out:
- The unused `sections = []` accumulator on lines 31-33. It's a separate
  seeded defect marked `# noqa: F841`, and CONTRIBUTING says not to
  clean those up.
- The `var-annotated` mypy suppression in `pyproject.toml`, which covers
  that same line, not this bug.
- Turning array items into separate sections. That's new behavior the
  issue doesn't ask for.
- Any change to the regex or to how JSON is located in the output.

## Files

- `rag/generator/output_parser.py`: `parse_review_output` (lines 39-48),
  add an `isinstance(data, dict)` check before each
  `_parse_json_output(data)` call.
- `tests/unit/test_output_parser.py`: delete the
  `@pytest.mark.xfail(...)` marker on `test_json_array_fallback` (lines
  141-144), and add `test_json_array_in_code_fence` to cover the fence
  path, which has the same bug.

## Approach

1. In the code-fence branch: after `data = json.loads(json_str)`, return
   `_parse_json_output(data)` only if `data` is a dict; otherwise fall
   through to the next step.
2. In the raw-JSON branch: same check. A non-dict value falls through to
   `_parse_plaintext_output(raw)`.
3. Remove the `xfail` marker from `test_json_array_fallback`.
4. Add `test_json_array_in_code_fence`: a fenced ```` ```json ```` array,
   asserting a list with one `general_feedback` section and no
   exception.
5. Run `make check && make test-unit`.

## Test plan

Re-run the unit 2 repro against the change:

```
.venv/bin/python -c 'from rag.generator.output_parser import parse_review_output; print(parse_review_output("[\"First feedback item\", \"Second feedback item\"]"))'
```

- Before (from my repro): `AttributeError: 'list' object has no
  attribute 'items'`.
- Expected after: no exception. The result is a list containing one
  `FeedbackSection` with `section_name="general_feedback"`,
  `confidence=0.7`, and the raw array text as `content`.

Then:
- `pytest tests/unit/test_output_parser.py -v` should show
  `test_json_array_fallback PASSED`, not `XFAIL`. If the marker were
  left in, it would show as a strict XPASS failure. The new fence test
  should also pass.
- Existing JSON-object tests (for example
  `test_json_wrapped_in_code_fence`) should still pass unchanged, which
  shows dict output still goes through the JSON path.
- `make check && make test-unit` should both be green.

## Risks and unknowns

- Falling back to plain text keeps the whole array as one section's
  content. Callers that expect one section per feedback point won't get
  that. I'm taking the simplest fix the issue and the test allow ("May
  fall back to plaintext or handle specially"). If a maintainer prefers
  one section per array item, that's a follow-up.
- I haven't checked whether any caller of `parse_review_output` relies on
  the crash, for example by catching `AttributeError`. I'll grep for
  callers before building.

## Deviations

Built as planned: added the `isinstance(data, dict)` check at both call
sites, removed the xfail marker, and added `test_json_array_in_code_fence`.
No changes to the plan's scope.

Checked the risk I listed: the only caller is `review_generator.py`
line 76, which takes `sections[0]` and doesn't catch `AttributeError`,
so it now gets one plain-text section instead of a crash.

Environment note, not a code change: my venv runs Python 3.13, so
`make typecheck` failed inside numpy's type stubs before reaching our
code. I ran mypy with `--python-version 3.12` locally instead; CI uses
3.11 and isn't affected.
