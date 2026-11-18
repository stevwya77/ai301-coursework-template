# Reproducing and Resolving an OpenImageIO Memory Leak

## Overview

OpenImageIO (OIIO) is an open-source library for reading, writing, and processing image files, with Python bindings that expose its C++ objects. [Issue #4613](https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613) reported that converting EXR frames to PNG with the Python bindings increased RAM use by roughly one gigabyte per frame. I investigated the report by improving the issue-scout rubric, claiming #4613, reproducing the behavior across releases and operating systems, and [reporting my findings](https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940). lgritz accepted the evidence and closed #4613, linking the resolution to pull request #4685. The investigation also exposed a separate Python binding bug, which I filed as [#5698](https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/5698). I also added repo-in-use and accepts-outside-prs checks to the scout and wrote a voice guard to keep public comments specific and free of unsupported claims.

## What happened

### 1. Fixing the scout before using it (Oct 30 – Nov 11)

The scout initially looked only for AI-use policy restrictions, so it could accept a repository with no contribution-policy problem. That was exposed by the Ladybird case: its `CONTRIBUTING.md` says, “We do not accept pull requests from non-maintainers.” Because the old checks did not inspect this, a clean Ladybird issue could pass. After adding the missing rules, the scan changed from accepting the flawed candidates to 0 accepted and 5 rejected; every rejection was for repo-in-use, with repositories ranging from 2, 13, 0, 0, and 0 stars.

```text
| repo-in-use | ... fails if the repo has fewer than 20 stars or was created less than 90 days before the capture date ... | required |
| accepts-outside-prs | ... fails if the stated policy says the repo does not accept pull requests from outside contributors ... | required |
```

### 2. Scout and claim (Nov 11)

The scout found an accept verdict on OpenImageIO #4613 with all six checks passing: 2,391 stars, a maintainer reply within about 1.5 hours on a recent issue, no existing claims or pull requests, a triaged bug, willingness to accept outside pull requests, and an AI policy that required disclosure rather than a prohibition.

```text
stevwya77: Hi, I'd like to work on this. I'll start by checking whether the leak
still happens on a current version and narrowing down where it occurs, as you
suggested. I'll report back what I find.

lgritz (Collaborator): @stevwya77 Sounds good!
```

### 3. The repro (Nov 14)

I tested OpenImageIO 3.0.3.1, 3.0.5.0, 3.0.6.0, and 3.2.2.0 on Windows, Linux, and macOS. The matrix contained 12 jobs, and [CI run 43636130390](https://github.com/stevwya77/oiio-4613-repro/actions/runs/43636130390) completed successfully across Windows and Linux. The probe ran the image generator in a subprocess because its allocations do not affect the RSS of the Python process being measured.

```text
Windows, OIIO 3.0.5.0:
png comp=0 filter=8 rgba half imageinput  base: 56 | d1: +324 | d2: +320 | ... | final: 3260
tif comp=0 filter=8 rgba half imageinput  base: 56 | d1:   +5 | d2:   +0 | ... | final:   18

Windows, OIIO 3.0.6.0:
png comp=0 filter=8 rgba half imageinput  base: 56 | d1:   +4 | d2:   +0 | ... | final:   60

Linux, OIIO 3.0.5.0:
png comp=0 filter=8 rgba half imageinput  base: 73 | d1: +321 | d2: +320 | ... | final: 3275
tif comp=0 filter=8 rgba half imageinput  base: 73 | d1:   +2 | d2:   +2 | ... | final:   78

Linux, OIIO 3.0.6.0:
png comp=0 filter=8 rgba half imageinput  base: 73 | d1:   +1 | d2:   +0 | ... | final:   75
```

RSS values are in MiB.

### 4. Five bytes per value

The growth scale matched the RGBA image's value count. The extra roughly five bytes for every half-float value was consistent with a Python object-lifetime problem rather than PNG encoding itself.

```text
png ... rgba half  -> mean delta +320.2 MiB (Final: 3249)
png ... rgb  half  -> mean delta +240.1 MiB (Final: 2448)

8192 x 2048 x 4 = 67,108,864 values x 5 B = 320.0 MiB
8192 x 2048 x 3 = 50,331,648 values x 5 B = 240.0 MiB
```

### 5. RSS vs. footprint on macOS

On macOS, RSS fell from roughly +403 MiB to +2 MiB at 3.0.3.1, while `phys_footprint` continued to increase by about 320 MiB. That showed that RSS alone was not a complete memory measurement. One possible explanation is that the kernel compressed or reclaimed old physical pages, though I did not verify that mechanism.

```text
psutil RSS:            png rgba half -> d1 +110 ... mean +5.5 -> final 215
macOS phys_footprint:  png rgba half -> d1 +1   ... mean 0.0  -> final 45
```

### 6. #4685

The fix was merged on 2025-04-07, after 3.0.5.0 (released 2025-04-02) and before 3.0.6.0 (2025-05-01). Issue #4613 did not explicitly link to it. Pull request #4685 changed `py_imageoutput.cpp` so ownership stayed with a `unique_ptr`, allowing pybind11 to manage lifetime and represent an uninitialized pointer as `None`.

```diff
// src/python/py_imageoutput.cpp
-            [](const std::string& filename,
-               const std::string& searchpath) -> py::object {
-                auto out(ImageOutput::create(filename, nullptr, searchpath));
-                return out ? py::cast(out.release()) : py::none();
+            [](const std::string& filename, const std::string& searchpath) {
+                return ImageOutput::create(filename, nullptr, searchpath);
```

### 7. The probe finds a second bug

While building the reproduction, the probe found a separate failure in OIIO 3.2.2.0: a small in-memory call returned `NoneType` with exit 0, while four big lazy file-backed cases were killed by a segmentation fault.

```text
OIIO 3.2.2.0
  [                exit 0]  1 small in-memory, UNKNOWN   -> NoneType  | err: -
  [killed by SIGSEGV (11)]  4 big lazy file, UNKNOWN     Fatal Python error: Segmentation fault
```

### 8. repro-check

The reproduction validator returned an accept verdict, but it identified two editorial corrections rather than evidence problems.

```text
Verdict: accept. All five required checks pass, and so does the preferred control-run check.

Things to fix before posting (none of them change the verdict):
1. Linux has no output in the report.
2. The 3.2.2.0 explanation is written as fact.
```

The checker also identified unsupported cause speculation in three sections, so the report was edited to mark the macOS interpretation and 3.2.2.0 behavior explicitly as uncertain.

### 9. My voice guard caught two lines (Nov 18)

The first guard stopped the post because it offered work without identifying a concrete first step and report-back date.

```text
2026-11-18 02:19 [manual] stop: rule broken: "Never offer or claim work without naming, in the same
comment, a specific first step I can start today and the date I will report back. A claim never
promises a fix, ..." | target: issue.md
```

Before:

```text
Found while writing a reproduction for #4613; happy to work on a fix.
```

After (as posted in #5698):

```text
Found while writing a reproduction for #4613.
```

A second guard stopped a suspected root-cause claim before I had traced the exact code.

```text
2026-11-18 02:28 [manual] stop: rule broken: "Unless I have traced the exact line of code and am
opening a PR, I will not speculate on the root cause." | target: issue-ready.md
```

Deleted line:

```text
That pattern suggests an out-of-bounds access rather
than a clean "unsupported format" failure.
```

The guard log also recorded a third STOP at 04:21 on `comment.md` for subjective words such as “latest” or “newest” in environment descriptions; the corrected version passed.

### 10. Checking it myself: 0/10, then 10/10

First loop: write and read in the same process, 10 runs:

```text
returned NoneType
run 1 exit 0
returned NoneType
run 2 exit 0
...
returned NoneType
run 10 exit 0
```

Second loop: write the file once, then read it in a fresh process each time, the way `probe.py` does:

```text
Segmentation fault: 11
run 1 exit 139
Segmentation fault: 11
run 2 exit 139
...
Segmentation fault: 11
run 10 exit 139
```

What I posted in #5698 after that:

```text
On a file-backed buffer opened in a fresh process with no `read()`, it segfaulted in 10 of 10 runs.
```

The earlier same-process behavior was inconsistent: Claude Code showed three crashes, while a later controlled check showed `None` in all 10 runs. That evidence was cut because it was not reliable. Afterward, three fresh-process runs confirmed three more segfaults, bringing that fresh-process result to 13/13.

Guard-malloc, Nov 14 vs. Nov 18:

```text
Nov 14 (probe-output.txt):
  [ killed by SIGABRT (6)]  2 big in-memory, UNKNOWN    Fatal Python error: Aborted
  [ killed by SIGABRT (6)]  4 big lazy file, UNKNOWN    ... std::__1::future_error: Operation not permitted ...

Nov 18 (fresh run, posted in #5698):
[killed by SIGSEGV (11)]  2 big in-memory, UNKNOWN    Fatal Python error: Segmentation fault
[killed by SIGSEGV (11)]  4 big lazy file, UNKNOWN    Fatal Python error: Segmentation fault
```

### 11. Filing #5698 and reporting back on #4613

The reproducible second bug was documented as [OpenImageIO #5698](https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/5698) with a minimal Python example:

```python
import OpenImageIO as oiio
# Step 2: in a NEW Python process, read it lazily
oiio.ImageBuf('big.exr').get_pixels(oiio.UNKNOWN)       # SIGSEGV, 10 of 10 runs
```

I then posted the report-back to [#4613](https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940):

> The leak is there in the wheels I tested through 3.0.5.0 (3.0.3.1 and 3.0.5.0) and is gone from 3.0.6.0 onward. This holds on Windows x64, Linux x64 and macOS arm64. 3.0.6.0 is the first release that contains #4685.

### 12. How it ended

lgritz replied that he would close the issue and cited #4685 as the fix. For #5698, he set the issue type to Bug and added the `bug`, `python`, and `good first issue` labels. OIIO's AI policy discourages AI tools for fixing “good first issue” issues, so that policy will affect how I approach #5698 next.

> Thanks for checking. I will close this issue, then.
>
> Fixed by #4685
>
> — lgritz, closing #4613

## Limitations and what's next

The investigation did not bisect the versions between 3.0.5.0 and 3.0.6.0, so it cannot identify the exact first fixed release within that interval. #5698 remains unfixed, and the scout's ranking still surfaces unusable candidates; a later scan note said that “the ranking still feeds it the same losers.” The next step is to claim #5698 and fix it myself, while following the project's AI disclosure and good-first-issue policies.

> Thanks for checking. I will close this issue, then.
>
> Fixed by #4685
>
> — lgritz
