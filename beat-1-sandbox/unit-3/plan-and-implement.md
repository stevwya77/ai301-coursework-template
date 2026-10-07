# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

stevwya77

**Plan comment**
```
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-6029846478

Plan for this one, building on my repro above (the top-level array
raises AttributeError: 'list' object has no attribute 'items' inside
_parse_json_output).

The cause is that parse_review_output hands any decoded JSON to
_parse_json_output, which assumes a dict. My fix is to check for a dict
at both call sites (the code-fence path and the raw-JSON path) and let
anything else fall through to the existing plain-text fallback. I'll
also remove the xfail marker on test_json_array_fallback and add a
fenced-array test, since the fence path has the same bug.

Not touching: the sections = [] line marked # noqa (that's a
separate seeded defect), the JSON extraction regex, or splitting array
items into separate sections.

I'll know it's fixed when my repro returns one general_feedback
section instead of raising, test_json_array_fallback passes without
the marker, and make check && make test-unit is green. Branch:
fix/69-json-array-fallback
```
---

## Your branch

**Branch**
```
fix/69-json-array-fallback
```
**Evidence**
```
cat ~/issue69-before.txt

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

```
cat ~/issue69-after.txt
2026-10-06 23:02:10 [info     ] plaintext_output_parsed        content_length=47
[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]
============================= test session starts ==============================
platform darwin -- Python 3.13.7, pytest-9.1.1, pluggy-1.6.0 -- /Users/stephanie/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /Users/stephanie/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: platformdirs-4.12.2, hypothesis-6.168.3, cov-7.1.0, asyncio-1.4.0, benchmark-5.3.0, pytest_httpserver-1.1.5, anyio-4.15.1
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 20 items

tests/unit/test_output_parser.py::TestOutputParser::test_json_wrapped_in_code_fence PASSED [  5%]
tests/unit/test_output_parser.py::TestOutputParser::test_raw_json_without_fence PASSED [ 10%]
tests/unit/test_output_parser.py::TestOutputParser::test_plain_text_fallback PASSED [ 15%]
tests/unit/test_output_parser.py::TestOutputParser::test_malformed_json_fallback PASSED [ 20%]
tests/unit/test_output_parser.py::TestOutputParser::test_feedback_section_has_required_fields PASSED [ 25%]
tests/unit/test_output_parser.py::TestOutputParser::test_json_with_multiple_sections PASSED [ 30%]
tests/unit/test_output_parser.py::TestOutputParser::test_confidence_scores_in_sections PASSED [ 35%]
tests/unit/test_output_parser.py::TestOutputParser::test_empty_json_object PASSED [ 40%]
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback PASSED [ 45%]
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_in_code_fence PASSED [ 50%]
tests/unit/test_output_parser.py::TestOutputParser::test_very_long_plain_text PASSED [ 55%]
tests/unit/test_output_parser.py::TestOutputParser::test_html_in_feedback PASSED [ 60%]
tests/unit/test_output_parser.py::TestOutputParser::test_code_in_feedback PASSED [ 65%]
tests/unit/test_output_parser.py::TestOutputParser::test_section_suggestions_extraction PASSED [ 70%]
tests/unit/test_output_parser.py::TestOutputParser::test_unicode_characters PASSED [ 75%]
tests/unit/test_output_parser.py::TestOutputParser::test_nested_json_structure PASSED [ 80%]
tests/unit/test_output_parser.py::TestOutputParser::test_mixed_content PASSED [ 85%]
tests/unit/test_output_parser.py::TestOutputParser::test_plaintext_output_helper PASSED [ 90%]
tests/unit/test_output_parser.py::TestOutputParser::test_multiple_code_fences PASSED [ 95%]
tests/unit/test_output_parser.py::TestOutputParser::test_no_suggestions_key PASSED [100%]

============================== 20 passed in 0.20s ==============================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**
```
categories: clear-accept 5/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4
agreement: 18/20 scored items  (bar: 18/20: PASS)

categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4
agreement: 19/20 scored items  (bar: 18/20: PASS)
```
**Package analysis**
```
pkg-08, Gold was accept. It was initially rejected it on honesty because "confirmed by the repro controls" read as overclaiming, even though the controls back it up. This led me to revise the check.
```
**Check rationale**
```
"passes only if every certainty claim is backed by something the Repro evidence or Thread highlights shows; confident wording is fine when the evidence supports it; fails only claims the evidence doesn't show and the plan doesn't name as an unknown"
it judges whether the evidence supports a claim, and not only on how confident the wording sounds.
```

**Trade-offs**
```
A looser honesty check can't flag a confident claim the grader thinks the evidence supports. If you notice, my second run showed me that one check can hide a gap in another.
```
---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
