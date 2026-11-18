# Contribution: bug: Python ImageBuf.get_pixels(oiio.UNKNOWN) returns None silently or segfaults

<!-- Original issue: #4613, "Frame on Frame RAM Increase Using PyBindings for 3.1.0 Converting EXR To Uncompressed PNG".
     Course chat approved switching the contribution to #5698 on 2026-11-18. -->

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]

**Student:** Stephanie Wyatt

**GitHub Username:** stevwya77

**Issue:** https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/5698 (found while reproducing https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613, which lgritz closed as fixed by #4685 on 2026-11-18)

**Pull Request:** [GitHub PR link, once opened]


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

**Unit 5: Built issue-scout, a skill that searches GitHub from my fit profile, ranks the results, and grades the top five against my rubric**

I upgraded my Unit 1 issue-select skill into issue-scout by turning my fit profile into gh search queries and ranking rules, then tested it with five scans. The scans exposed real gaps: my rubric passed repos that ban AI-written code, refused outside PRs, or had almost no users, and my ranking kept surfacing hours-old issues in toy repos instead of my priority projects. I added repo-in-use and accepts-outside-prs checks, tightened unclaimed and newcomer-scope to catch upstream bugs, missing hardware, and maintainers fixing issues themselves, and reordered the ranking so my priority repos came first. By the fifth scan, every candidate came from an active project and every rejection I checked was correct.

**Unit 6: Built a voice guard and checked whether repo memory was available**

I installed the Unit 6 starter and added an Upstream section to my voice guide before drafting issue and comment text. The guard stopped issue.md, issue-ready.md, and comment.md drafts on 2026-11-18: it rejected a claim that offered work too broadly, removed a speculative root-cause statement before I had traced the exact code, and caught subjective wording such as “latest” or “newest.” I revised each draft to remain specific, factual, and evidence-based. I could not verify a repo memory file: CLAUDE.local.md was not found or confirmed, so I did not claim that repo memory was working.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my issue-scout skill, which turns my fit profile (C++ and Go first, bug fixes and tests, projects like GrapheneOS, Mayhem, Zulip, Kubernetes, and the EFF) into GitHub searches, ranks the results, and grades the top five against my rubric. Across six scans it graded thirty issues, and almost all were correctly rejected: one repo banned AI-written code, one refused outside PRs, others were upstream bugs, already claimed, or in tiny brand-new repos. I also graded my picks from the course's Issue Selection site, and the scout accepted OpenImageIO #4613 on all six checks. It fits my profile best: it is a C++ bug in an active project with over 2,000 stars, a maintainer (lgritz) labelled it a good first issue and invited newcomers to confirm the leak and narrow down its source, I can reproduce it on my Mac without special hardware, and the project's AI policy asks for a human author and an Assisted-by line. I hoped to learn how to trace a memory leak across Python bindings and C++ code. The maintainer warned that the full fix "may or may not" be newcomer-sized, so my first goal was a solid reproduction and diagnosis.

Honest read of the scout: the rubric held up. It caught everything I built it to catch, and every rejection I checked was correct. The ranking is the weak part. Ranking my priority repos first keeps filling the top five with unlabelled issues from those repos, so my latest scan graded five issues and none were usable. It also assumed my operating system until I listed it in my profile. Next I'd add label-based drop rules and require beginner labels on priority-repo queries.

Claim comment: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6788821588

Claim comment (#5698): claim pending, 2026-11-18

On 2026-11-14, probe.py found `get_pixels(oiio.UNKNOWN)` returning `None` or segfaulting. On 2026-11-18, I posted my report-back on #4613. lgritz replied, "Thanks for checking. I will close this issue, then. / Fixed by #4685," so #4613 was closed with no fix left to make. I filed #5698 that day; lgritz set it to Bug and added the bug, python, and good first issue labels. The course chat approved #5698 as the contribution. OpenImageIO discourages AI tools for fixing good first issues, so I will implement and understand the fix myself rather than ask a coding agent to submit it.

**Toolkit**

- Tool: issue-scout, my search strategy and rubric. I ran six scans plus a grade run on my Issue Selection picks.
- What it produced: an accept verdict on OpenImageIO #4613 with all six checks passing (2,391 stars, maintainer reply within ~1.5 hours on a recent issue, no claims or PRs, a triaged single bug, outside PRs welcome, AI allowed with disclosure).
- Difficulties: the ranking kept sending unlabelled priority-repo issues to grading, so my last scan found nothing usable; GitHub rate-limited several queries; the scout guessed my OS until I listed it.
- What changed: I added repo-in-use and accepts-outside-prs checks, tightened unclaimed and newcomer-scope, and listed my machines in my profile. Next I'd add label-based drop rules and require beginner labels on priority-repo queries.
---

## Understanding the Issue

**Problem Description**

When a Python script uses OpenImageIO's Python bindings to read a large EXR and write it as an uncompressed PNG, memory usage climbs with every image and is never released. The reporter (OpenImageIO 3.1.0, Python 3.10.6, Windows) saw about 1 GB added per frame on roughly 20,000 × 2,000 images, so a batch job eventually runs out of RAM. The same conversion with oiiotool doesn't leak, and writing TIFF instead of PNG from Python doesn't leak, which pointed at the Python PNG write path.

**Expected Behavior**

Memory should stay roughly flat across frames. Once a frame's ImageOutput is closed and the pixel data goes out of scope, its memory should be freed before the next frame starts.

**Current Behavior**

Each converted frame can add memory that is not released, so usage grows steadily until the process is stopped. The maintainer asked whether this still happens in current versions.

Update, 2026-11-18: confirmed. The leak is present in 3.0.3.1 and 3.0.5.0 and gone from 3.0.6.0 onward on Windows x64, Linux x64 and macOS arm64. lgritz closed #4613 as fixed by #4685.

#5698, current behavior: `ImageBuf.get_pixels(oiio.UNKNOWN)` returns `None` with no error on in-memory and small file-backed buffers. On a file-backed buffer opened in a fresh process with no `read()` call, it segfaults in 10 of 10 runs. The expected behavior is for `UNKNOWN` to act as the buffer's native pixel type, like `ImageBuf.read()` does, or for a Python exception to make the invalid request explicit; it should never silently return `None` or crash.

**Affected Components**

The original PNG-plugin guess was wrong. #4685 fixed the leak in the Python binding for `ImageOutput.create` (`src/python/py_imageoutput.cpp`), not in `src/png.imageio/`.

#5698 is in the Python `ImageBuf` binding for `get_pixels` (`src/python/py_imagebuf.cpp`). The binding calls C++ `ImageBuf::get_pixels` (`src/libOpenImageIO/imagebuf.cpp`) and NumPy array construction (`src/python/py_oiio.cpp`). `TypeDesc::UNKNOWN` is passed through as the requested format, so the allocation and dispatch are the key areas to trace.

**Toolkit**

- Tool: issue-scout's grade read-out, plus source tracing and the issue thread.
- What it produced: a summary of the bug, the maintainer's triage note, and confirmed file and function locations.
- Difficulties: the original component guess was wrong; source tracing is still needed to choose and implement the fix.
- What changed: I corrected the affected component for #4613 and narrowed #5698 to `py_imagebuf.cpp`, C++ `get_pixels`, and NumPy array construction.
---

## Reproduction Process

**Environment Setup**

- Local: macOS 27.2.1 (arm64), Python 3.10.7 in a venv, numpy 2.2.6, psutil 7.2.2.
- OpenImageIO from PyPI wheels (`pip install OpenImageIO==<version>`): 3.0.3.1, 3.0.5.0, 3.0.6.0, 3.2.2.0. No build from source.
- CI: GitHub Actions, `ubuntu-latest`, `windows-latest`, `macos-latest`, Python 3.10, the same four wheels: 3 OS × 4 versions = 12 jobs.
- Repro code: https://github.com/stevwya77/oiio-4613-repro (`repro.py` for #4613, `probe.py` for #5698).

Challenges and how I handled them:

- On macOS, RSS stopped showing the leak: per-frame RSS growth fell from +403 MB to +2 MB on the 3.0.3.1 CI job while `phys_footprint` kept adding about +320 MB. `repro.py` records both.
- Generating the large EXR inside the measured process would add its own memory, so `repro.py` generates it in a separate process.
- For #5698, writing and then reading the file in the same process returned `None` in 10 of 10 runs in my terminal, while `probe.py` had crashed. `probe.py` opens the file in a fresh process; doing the same gave segmentation faults in 10 of 10 runs.

**Steps to Reproduce**

#4613 (memory growth):

1. `pip install OpenImageIO==3.0.5.0 numpy psutil`
2. `python repro.py --matrix --iters 10 --only issue --only tif-control --only no-alpha --only float-in`
3. Observed: the PNG issue configuration adds about +320 MB per iteration; the TIFF control stays flat. Repeating with `OpenImageIO==3.0.6.0`, the PNG configuration stays flat.

#5698 (`get_pixels(oiio.UNKNOWN)`):

1. `pip install OpenImageIO==3.2.2.0`
2. In one Python process: `b = oiio.ImageBuf(oiio.ImageSpec(4, 4, 3, oiio.HALF)); print(b.get_pixels(oiio.UNKNOWN), repr(b.geterror()))`
3. Observed: `None ''` (no exception, no error).
4. In one process, write a 2048×2048×4 HALF image: `b = oiio.ImageBuf(oiio.ImageSpec(2048, 2048, 4, oiio.HALF)); oiio.ImageBufAlgo.zero(b); b.write('big.exr')`
5. In a new Python process: `oiio.ImageBuf('big.exr').get_pixels(oiio.UNKNOWN)`
6. Observed: `Segmentation fault: 11` (exit 139) in 10 of 10 runs.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/stevwya77/oiio-4613-repro/commit/3838d62ffbe2b8ec68a1a7a851a39caa4fabc972 (#4613 CI matrix bracketing #4685) and https://github.com/stevwya77/oiio-4613-repro/commit/21e3d2a0673d64ce8dfc7e2b81dcb454be54ce66 (`probe.py` for #5698)
- **Screenshots/logs:** CI run https://github.com/stevwya77/oiio-4613-repro/actions/runs/43636130390 (12 jobs); probe output in #5698.

```text
Windows, OIIO 3.0.5.0 (RSS, MB):
png comp=0 filter=8 rgba half imageinput  base 56  d1 +324  d2 +320 ... final 3260
tif comp=0 filter=8 rgba half imageinput  base 56  d1   +5  d2   +0 ... final   18

Windows, OIIO 3.0.6.0:
png comp=0 filter=8 rgba half imageinput  base 56  d1   +4  d2   +0 ... final   60
```

- **My findings:**
  - The leak is 5 bytes per pixel value per frame: +320 MiB per frame for 8192×2048 RGBA (67,108,864 values × 5 B) and +240 MiB for RGB. Half and float input leak the same amount.
  - It is gone from 3.0.6.0, the first release that contains #4685 (merged 2025-04-07). I reported this on #4613 (https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940), and lgritz closed it.
  - With #4613 already fixed, there was no fix left to make. While reproducing it, `probe.py` found the `get_pixels(oiio.UNKNOWN)` bug, which I filed as #5698.

**Toolkit**

- Tool: repro-check (wild scope), run on my claim and repro draft on 2026-11-14; my voice guard (`check_voice.py --draft`) on the #5698 issue draft and #4613 report-back on 2026-11-18.
- What it produced: repro-check accepted the claim and reproduction and identified fixes needed in the Linux output and in how I described the 3.2.2.0 behavior. Voice guard produced three stops and prevented three draft claims or guesses.
- Difficulties: repro-check could accept guessed explanations when they were phrased as guesses, while my voice guide forbids speculative root-cause claims. The guard only checked drafts when explicitly run with `--draft`.
- What changed: I cut the stopped claim and speculative root-cause wording. I reran the crash step in a fresh process, updated the evidence, and left out the outdated cause explanations.
---

## Solution Approach

**Analysis**

`ImageBuf_get_pixels` in `src/python/py_imagebuf.cpp` uses the requested format's `size()` to allocate its temporary buffer before calling C++. `oiio.UNKNOWN` has no storage size, so the allocation is invalid for pixel data. The C++ and NumPy dispatch paths also do not recognize `UNKNOWN` as a concrete output type. This produces a silent `None` for in-memory or small file-backed buffers and can leave dispatch with a destination range that is unsafe for a large lazy, file-backed buffer, explaining the fresh-process crash.

The binding registers `get_pixels` with `TypeFloat` as its default in `py_imagebuf.cpp` lines 521–522. It is therefore explicitly passing `UNKNOWN` that changes behavior from the normal `TypeFloat` path. `ImageBuf.read(..., convert=TypeUnknown)` in `py_imagebuf.cpp` lines 380–387 establishes the useful native-type convention: `UNKNOWN` means to use the buffer's native format rather than inventing a zero-byte destination.

The C++ implementation in `src/libOpenImageIO/imagebuf.cpp` lines 2717–2763 validates and dispatches a destination ROI. `TypeDesc::size()` in `typedesc.h` lines 203–218 shows that `UNKNOWN` is not a concrete storage format, and `make_numpy_array` in `src/python/py_oiio.cpp` lines 338–376 returns `None` for unsupported formats rather than allocating a concrete NumPy dtype.

**Proposed Solution**

The implementation should normalize `UNKNOWN` before allocation and dispatch. One approach is to resolve it to `buf.pixeltype()` or `buf.nativespec().format`, matching the native-format convention of `ImageBuf.read()`. The selected format must be concrete and supported by `make_numpy_array`; the returned array should have that native dtype and the requested ROI shape. An alternative, if maintainers prefer, is to reject `UNKNOWN` explicitly with a Python exception instead of silently allocating or dispatching. I will choose the behavior after checking repository conventions and project discussion; for now, I am recording the problem and implementation boundary without claiming a fix.

Deliberately not changing: the C++ API's handling of explicit non-UNKNOWN formats, the default `TypeFloat` behavior, ROI semantics, NumPy shape conventions, or unrelated Python binding code. A fix should stay in the Python `get_pixels` binding and its minimal native-format normalization unless C++ tracing proves that the core dispatch itself is wrong.

**Implementation Plan**

1. Modify `ImageBuf_get_pixels` in `src/python/py_imagebuf.cpp` to resolve `UNKNOWN` to a concrete native format before allocating and dispatching.
2. Add in-memory, small file-backed, and lazy large file-backed regression coverage near the existing `get_pixels` test in `testsuite/python-imagebuf/src/test_imagebuf.py`.
3. Update `testsuite/python-imagebuf/ref/out.txt` only if the test's printed output changes, and run `testsuite/python-imagebuf/run.py` plus targeted reproduction steps.
4. Run source formatting and the relevant testsuite or build checks required by the repository.

I have not yet begun source edits or test runs, so there are no implementation deviations to record yet.

**Toolkit**

- Tool: direct source tracing of the pinned main branch and comparison with the existing Python binding conventions.
- What it produced: concrete file/function locations and a native-format normalization plan.
- Difficulties: I have not chosen between native-format conversion and explicit exception behavior, and I have not run a patched build to verify either approach.
- What changed: I narrowed the fix boundary from the whole binding to `py_imagebuf.cpp` plus, if needed, focused NumPy/C++ helpers; no code is changed yet.
---

## Testing Strategy

Before the change, OIIO 3.2.2.0 returned `None` for an in-memory 4×4×3 HALF buffer and crashed a fresh process for a 2048×2048×4 lazy file-backed HALF buffer. The in-memory and small-file cases have no error text.

The observable checks after implementation will be:

1. Re-run the exact in-memory reproduction and assert it does not return `None` silently. The intended result is either a NumPy array in the resolved native dtype or an explicit Python exception that states what is unsupported.
2. Re-run the exact fresh-process large-file reproduction and verify that it exits normally instead of with SIGSEGV.
3. Re-run `get_pixels(oiio.HALF)` and `get_pixels()` as controls and verify that their existing arrays and shapes remain unchanged.
4. Add tests to `testsuite/python-imagebuf/src/test_imagebuf.py`, which is already included in the Python ImageBuf test script, and run `testsuite/python-imagebuf/run.py` to compare generated output with `testsuite/python-imagebuf/ref/out.txt`.
5. Run the repository build and test commands documented in `INSTALL.md` and `CONTRIBUTING.md`, including relevant source formatting and clean test-suite execution. Record actual commands and outcomes rather than reporting success from assumption.

No post-change test outcome is claimed yet.

**Toolkit**

- Tool: before-change reproduction scripts and the Python ImageBuf testsuite links.
- What it produced: a before-state with observable `None`, crash, and control behavior; a testing plan that names exact files and output comparisons.
- Difficulties: no patched build or test run has happened yet, and platform-specific crash outcomes may differ from Linux or Windows until run there.
- What changed: I separated expected behavior from verified outcomes and will add only test results that I actually run.
---

## Implementation Notes

**2026-11-11**

issue-scout accepted #4613 on all six checks. I claimed it; lgritz replied "@stevwya77 Sounds good!".

**2026-11-13 – 2026-11-14**

Wrote `repro.py` and a CI matrix (3 OS × 4 wheels). Found the leak is 5 bytes per pixel value per frame. Switched to measuring `phys_footprint` on macOS after RSS stopped showing growth. Bracketed the fix between 3.0.5.0 and 3.0.6.0 and matched it to #4685. Wrote `probe.py`, which found `get_pixels(oiio.UNKNOWN)` returning `None` or crashing. repro-check accepted my claim and reproduction and listed fixes.

**2026-11-18**

The voice guard stopped three drafts (02:19, 02:28, 04:21) and I fixed each. Wrong turn: my crash snippet reproduced 0 of 10 times in my own terminal; opening the file in a fresh process, as `probe.py` does, gave 10 of 10 crashes. I changed the #5698 steps. Filed #5698 and posted my report-back on #4613. lgritz closed #4613 ("Fixed by #4685") and labelled #5698 `bug`, `python`, and `good first issue`. The course chat approved #5698 as my contribution.

**2026-11-18 onward**

Source tracing identified the concrete binding path: `py_imagebuf.cpp` allocates by `format.size()`, calls C++, and then passes the format to `make_numpy_array`. `UNKNOWN` is not a concrete size or NumPy dtype. No implementation or test patch has been made yet.

**Toolkit**

- Tool: issue-scout, repro-check, voice guard, CI logs, and direct source inspection.
- What it produced: an issue switch with evidence, a reproduction, a confirmed source location, and a record of wrong turns.
- Difficulties: the original work was based on #4613, while #5698 required a separate fresh-process probe and a new source trace.
- What changed: I switched the contribution after the maintainer closed #4613 and filed #5698 with the newly found regression.
---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** Awaiting implementation and PR opening.

**Toolkit**

- Tool: not yet used for PR submission or review.
- What it produced: no PR or maintainer response exists yet.
- Difficulties: implementation has not started and no PR can be truthfully described.
- What changed: nothing yet.
---

## Learnings and Reflections

**Technical Skills Gained**

Source tracing has shown the full handoff from a Python binding function through C++ ROI and pixel dispatch into NumPy array construction. I also learned to keep reproduced behavior separate from inferred root cause and to correct an earlier component guess when source evidence says otherwise.

**Challenges Overcome**

The most important process challenge was discovering that #4613 was already fixed and not making it my fallback claim. I switched to #5698 only after reproducing and reporting the new regression. I also corrected a fresh-process crash reproduction instead of relying on a snippet that had not crashed in my terminal.

**What I'd Do Differently Next Time**

I would record source line tracing and test output as soon as each result occurs, instead of reconstructing later. I would also add repo-memory verification to the Unit 6 record rather than leaving it uncertain.

---

## Resources Used

- OpenImageIO CONTRIBUTING.md, bug report template, and AI policy: https://github.com/AcademySoftwareFoundation/OpenImageIO/blob/main/docs/dev/AI_Policy.md
- Issue #4613 thread, including lgritz's triage note: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613
- PR #4685 (fix(python): fix handle leak): https://github.com/AcademySoftwareFoundation/OpenImageIO/pull/4685
- Issue #5698: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/5698
- Pinned source: https://github.com/AcademySoftwareFoundation/OpenImageIO/blob/a117412fe89f26ed61dc796b1e5367a3283c10c3
- Python ImageBuf tests: https://github.com/AcademySoftwareFoundation/OpenImageIO/blob/a117412fe89f26ed61dc796b1e5367a3283c10c3/testsuite/python-imagebuf/src/test_imagebuf.py
- Test runner: https://github.com/AcademySoftwareFoundation/OpenImageIO/blob/a117412fe89f26ed61dc796b1e5367a3283c10c3/testsuite/python-imagebuf/run.py
- Reference output: https://github.com/AcademySoftwareFoundation/OpenImageIO/blob/a117412fe89f26ed61dc796b1e5367a3283c10c3/testsuite/python-imagebuf/ref/out.txt
