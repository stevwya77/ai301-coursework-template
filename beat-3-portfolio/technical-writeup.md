# <!-- YOUR TITLE: says what you did --> 
 
## Overview
 
<!-- 4–5 sentences: what OIIO is · what #4613 was · what you did · how it ended (lgritz closed it, citing #4685)
     · you filed #5698 · the tool rules you wrote yourself (repo-in-use, accepts-outside-prs, voice guard).
     Link your report-back comment on #4613:
     https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940 -->
 
## What happened
 
### 1. Fixing the scout before using it (Oct 30 – Nov 11)
 
<!-- scan-notes.md:9 — Ladybird CONTRIBUTING.md: "We do not accept pull requests from non-maintainers."
     ai-policy only looked for AI bans, so a clean Ladybird issue would have passed.
     scan-notes.md:36 — after the patch: 0 accepted, 5 rejected, all on repo-in-use (2, 13, 0, 0, 0 stars). -->
 
```text
| repo-in-use | ... fails if the repo has fewer than 20 stars or was created less than 90 days before the capture date ... | required |
| accepts-outside-prs | ... fails if the stated policy says the repo does not accept pull requests from outside contributors ... | required |
```
<!-- from ~/.claude/skills/issue-scout/rubric.md:39 and :43, trimmed -->
 
### 2. Scout and claim (Nov 11)
 
```text
accept verdict on OpenImageIO #4613 with all six checks passing (2,391 stars,
maintainer reply within ~1.5 hours on a recent issue, no claims or PRs, a triaged
single bug, outside PRs welcome, AI allowed with disclosure)
```
 
```text
stevwya77: Hi, I'd like to work on this. I'll start by checking whether the leak
still happens on a current version and narrowing down where it occurs, as you
suggested. I'll report back what I find.
 
lgritz (Collaborator): @stevwya77 Sounds good!
```
 
### 3. The repro (Nov 14)
 
<!-- 3 OSes × 4 versions (3.0.3.1, 3.0.5.0, 3.0.6.0, 3.2.2.0). CI run 43636130390, all 12 jobs succeeded.
     Subprocess reason (repro-draft.md:21): the generator's allocations never touch the measured process. -->
 
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
<!-- RSS in MB, from CI run 43636130390 (windows-latest and ubuntu-latest jobs) -->
 
### 4. Five bytes per value
 
```text
png ... rgba half  -> mean delta +320.2 MiB (Final: 3249)
png ... rgb  half  -> mean delta +240.1 MiB (Final: 2448)
 
8192 x 2048 x 4 = 67,108,864 values x 5 B = 320.0 MiB
8192 x 2048 x 3 = 50,331,648 values x 5 B = 240.0 MiB
```
 
### 5. RSS vs. footprint on macOS
 
<!-- repro-draft.md:73: RSS deltas fell +403 → +2 on macOS CI 3.0.3.1 while footprint kept adding +320.
     Your guess, worded as a guess: the kernel compressing old pages. -->
 
```text
psutil RSS:            png rgba half -> d1 +110 ... mean +5.5 -> final 215
macOS phys_footprint:  png rgba half -> d1 +1   ... mean 0.0  -> final 45
```
 
### 6. #4685
 
<!-- merged 2025-04-07, between 3.0.5.0 (04-02) and 3.0.6.0 (05-01). Never linked to #4613. -->
 
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
 
```text
OIIO 3.2.2.0
  [                exit 0]  1 small in-memory, UNKNOWN   -> NoneType  | err: -
  [killed by SIGSEGV (11)]  4 big lazy file, UNKNOWN     Fatal Python error: Segmentation fault
```
<!-- ~/oiio-4613/probe-output.txt -->
 
### 8. repro-check
 
```text
Verdict: accept. All five required checks pass, and so does the preferred control-run check.
 
Things to fix before posting (none of them change the verdict):
1. Linux has no output in the report.
2. The 3.2.2.0 explanation is written as fact.
```
<!-- repro-check-run2-accept.txt, trimmed. It also flagged voice-guide breaks: three sections guess at causes. -->
 
### 9. My voice guard caught two lines (Nov 18)
 
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
 
```text
2026-11-18 02:28 [manual] stop: rule broken: "Unless I have traced the exact line of code and am
opening a PR, I will not speculate on the root cause." | target: issue-ready.md
```
 
Deleted line (from `~/oiio-4613/issue.md:15`):
 
```text
That pattern suggests an out-of-bounds access rather
than a clean "unsupported format" failure.
```
 
<!-- guard-log also shows a third STOP at 04:21 on comment.md: "Never use subjective words like
     "latest" or "newest" for environments", then a pass. Optional, but real. -->
 
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
 
Second loop: write the file once, then read it in a fresh process each time, the way probe.py does:
 
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
 
<!-- The draft also said "If the same process that wrote the file reads it back, it returns None instead
     (also 10 of 10)." It was cut before posting: earlier same-process runs under Claude Code had crashed
     3/3, so it wasn't reliable. A final check got 3 more segfaults in a fresh process (13/13 total). -->
 
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
 
```python
import OpenImageIO as oiio
# Step 2: in a NEW Python process, read it lazily
oiio.ImageBuf('big.exr').get_pixels(oiio.UNKNOWN)       # SIGSEGV, 10 of 10 runs
```
 
> The leak is there in the wheels I tested through 3.0.5.0 (3.0.3.1 and 3.0.5.0) and is gone from 3.0.6.0 onward. This holds on Windows x64, Linux x64 and macOS arm64. 3.0.6.0 is the first release that contains #4685.
 
<!-- #5698: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/5698
     #4613 comment: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940 -->
 
### 12. How it ended
 
> Thanks for checking. I will close this issue, then.
>
> Fixed by #4685
>
> — lgritz, closing #4613
 
<!-- #5698: lgritz set the issue type to Bug and added the labels bug, python, good first issue.
     Describe the labels; don't put words in his mouth.
     OIIO's AI policy discourages AI tools for fixing "good first issue" issues, which affects how you take on #5698 next. -->
 
## Limitations and what's next
 
<!-- Limits: no bisect between 3.0.5.0 and 3.0.6.0 · #5698 isn't fixed · the scout's ranking still surfaces
     unusable issues (scan-notes.md:40: "the ranking still feeds it the same losers").
     Next: claim #5698 and fix it yourself. -->
 
> Thanks for checking. I will close this issue, then.
>
> Fixed by #4685
>
> — lgritz
 
