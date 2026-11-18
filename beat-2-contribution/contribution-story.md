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
 
I upgraded my Unit 1 issue-select skill into issue-scout by turning my fit profile into gh search queries and ranking rules, then tested it with five scans. The scans exposed real gaps: my rubric passed repos that ban AI-written code, refuse outside PRs, or have almost no users, and my ranking kept surfacing hours-old issues in toy repos instead of my priority projects. I added repo-in-use and accepts-outside-prs checks, tightened unclaimed and newcomer-scope to catch upstream bugs, missing hardware, and maintainers fixing issues themselves, and reordered the ranking so my priority repos come first. By the fifth scan, every candidate came from an active project and every rejection was correct.
 
**Unit 6: [one line on the tool work]**
 
[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]
 
<!-- EVIDENCE (write your own 2-4 sentences, then delete this note):
     - Unit 6 starter cloned 2026-11-14; voice guard installed with an Upstream section in your voice guide.
     - ~/.claude/voice-guard/guard-log.md, 2026-11-18:
         02:19 stop on issue.md: claim rule ("Never offer or claim work without naming ... a specific first step ...
               and the date I will report back. A claim never promises a fix") -> line "happy to work on a fix" cut
         02:28 stop on issue-ready.md: root-cause rule ("Unless I have traced the exact line of code and am opening
               a PR, I will not speculate on the root cause") -> "suggests an out-of-bounds access" deleted
         04:21 stop on comment.md: subjective environment words ("latest"/"newest"), then a pass
     - Repo memory file (CLAUDE.local.md): NOT VERIFIED. Did you write one? If not, say so honestly. -->
 
**Unit 7: [one line on the tool work]**
 
[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]
 
**Unit 8: [one line on the tool work]**
 
[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]
 
---
 
## Why I Chose This Issue
 
I searched with my issue-scout skill, which turns my fit profile (C++ and Go first, bug fixes and tests, projects like GrapheneOS, Mayhem, Zulip, Kubernetes, and the EFF) into GitHub searches, ranks the results, and grades the top five against my rubric. Across six scans it graded thirty issues, and almost all were correctly rejected: one repo banned AI-written code, one refused outside PRs, others were upstream bugs, already claimed, or in tiny brand-new repos. I also graded my picks from the course's Issue Selection site, and the scout accepted OpenImageIO #4613 on all six checks. It fits my profile best: it's a C++ bug in an active project with over 2,000 stars, a maintainer (lgritz) labelled it good first issue and invited newcomers to confirm the leak and narrow down its source, I can reproduce it on my Mac with no special hardware, and the project's AI policy only asks for a human author and an Assisted-by line. I hope to learn how to track down a memory leak across the boundary between Python bindings and C++ code. The maintainer warned the full fix "may or may not" be newcomer-sized, so my first goal is a solid reproduction and diagnosis.
 
Honest read of the scout: the rubric held up. It caught everything I built it to catch, and every rejection I checked was correct. The ranking is the weak part. Ranking my priority repos first keeps filling the top five with unlabelled issues from those repos, so my latest scan graded five issues and none were usable. It also assumed my operating system until I added it to my profile. Next I'd add a drop rule for upstream, invalid, wontfix, duplicate, and ai-started labels, and require a good first issue or help wanted label on the priority-repo queries.
 
Claim comment: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6788821588
 
Claim comment (#5698): claim pending, 2026-11-18
 
<!-- EVIDENCE for a short paragraph of your own on the switch to #5698:
     - 2026-11-14: probe.py found get_pixels(oiio.UNKNOWN) returning None / segfaulting (probe-output.txt).
     - 2026-11-18: report-back on #4613 (https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940);
       lgritz: "Thanks for checking. I will close this issue, then. / Fixed by #4685" -> #4613 closed, no fix left to make.
     - 2026-11-18: filed #5698; lgritz set type Bug, labels bug, python, good first issue.
     - Course chat approved #5698 as your contribution.
     - Note OIIO AI policy: discourages AI tools for fixing "good first issue" issues -> you write the fix yourself. -->
 
**Toolkit**
 
- Tool: issue-scout, my search strategy and rubric. I ran six scans plus a grade run on my Issue Selection picks.
- What it produced: an accept verdict on OpenImageIO #4613 with all six checks passing (2,391 stars, maintainer reply within ~1.5 hours on a recent issue, no claims or PRs, a triaged single bug, outside PRs welcome, AI allowed with disclosure).
- Difficulties: the ranking keeps sending unlabelled priority-repo issues to grading, so my last scan found nothing usable; GitHub rate-limited several queries; the scout guessed my OS until I listed it.
- What changed: I added repo-in-use and accepts-outside-prs checks, tightened unclaimed and newcomer-scope, and listed my machines in my profile. Next I'd add label-based drop rules and require beginner labels on priority-repo queries.
---
 
## Understanding the Issue
 
**Problem Description**
 
When a Python script uses OpenImageIO's Python bindings to read a large EXR and write it as an uncompressed PNG, memory usage climbs with every image and is never released. The reporter (OpenImageIO 3.1.0, Python 3.10.6, Windows) saw about 1 GB added per frame on roughly 20,000 x 2,000 images, so a batch job over many frames eventually runs out of RAM. The same conversion with oiiotool doesn't leak, and writing TIFF instead of PNG from Python doesn't leak, which points at the PNG writer as called from Python.
 
**Expected Behavior**
 
Memory should stay roughly flat across frames. Once a frame's ImageOutput is closed and the pixel data goes out of scope, its memory should be freed before the next frame starts.
 
**Current Behavior**
 
Each converted frame adds about 1 GB of memory that is never freed, so usage grows steadily until the process is stopped. The maintainer asked whether this still happens in current versions, which hasn't been confirmed yet.
 
Update, 2026-11-18: confirmed. The leak is present in 3.0.3.1 and 3.0.5.0 and gone from 3.0.6.0 onward on Windows x64, Linux x64 and macOS arm64. lgritz closed #4613 as fixed by #4685.
 
#5698, current behavior: `ImageBuf.get_pixels(oiio.UNKNOWN)` returns `None` with no error on in-memory and small file-backed buffers. On a file-backed buffer opened in a fresh process with no `read()`, it segfaults (10 of 10 runs, 13 of 13 across two checks). Expected behavior, as filed in #5698: treat `UNKNOWN` as the buffer's native pixel type, as `ImageBuf.read()` does, or raise a Python exception; never return `None` without an error, and never crash.
 
**Affected Components**
 
Suspected, to confirm during reproduction: the Python bindings for ImageInput and ImageOutput (read_image and write_image), and the PNG output plugin in src/png.imageio/. The leak may be in how the bindings pass pixel buffers to the PNG writer, or in the PNG writer itself when compression is set to 0.
 
Update, 2026-11-18: the PNG-plugin guess was wrong. #4685 fixed the leak in the Python binding for `ImageOutput.create` (`src/python/py_imageoutput.cpp`), not in `src/png.imageio/`. #5698 is in the Python `ImageBuf` binding for `get_pixels` (`src/python/py_imagebuf.cpp`).
 
**Toolkit**
 
- Tool: issue-scout's grade read-out, plus Claude to help me read the issue thread and summarize it.
- What it produced: a summary of the bug, the maintainer's triage note, and a list of likely components to investigate.
- Difficulties: the components are guesses until I reproduce the leak and trace it; nobody has confirmed it still happens on current versions.
- What changed: nothing yet. Reproduction will confirm or correct this list.
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
- For #5698, writing and then reading the file in the same process returned `None` in 10 of 10 runs in my terminal, while `probe.py` had crashed. `probe.py` opens the file in a fresh process; doing the same gave segfaults in 10 of 10 runs, then 3 of 3. The #5698 report uses the fresh-process steps.
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
  - It is gone from 3.0.6.0, the first release that contains #4685 (merged 2025-04-07). I reported this on #4613 (https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940), and lgritz closed it: "Thanks for checking. I will close this issue, then. / Fixed by #4685".
  - With #4613 already fixed, there was no fix left to make. While reproducing it, `probe.py` found the `get_pixels(oiio.UNKNOWN)` bug, which I filed as #5698.
**Toolkit**
 
- Tool: repro-check (wild scope), run on my claim and repro draft on 2026-11-14; my voice guard (`check_voice.py --draft`) on the #5698 issue draft and #4613 report-back on 2026-11-18.
- What it produced: repro-check: "Verdict: accept. All five required checks pass, and so does the preferred control-run check." plus fixes (Linux output missing; the 3.2.2.0 explanation written as fact). Voice guard: three STOPs in `guard-log.md` (02:19, 02:28, 04:21).
- Difficulties: repro-check's rubric allowed cause-guesses worded as guesses, while my own voice guide says not to guess causes at all, so it accepted text my guide would reject. The voice guard only checks a draft when I run it with `--draft` before posting from the browser.
- What changed: I cut the lines the voice guard stopped: "happy to work on a fix" (claim rule) and "That pattern suggests an out-of-bounds access…" (root-cause rule). My #4613 report-back left out the cause explanations repro-check flagged.
---
 
## Solution Approach
 
**Analysis**
 
[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]
 
**Proposed Solution**
 
[High-level description of your fix approach, and what you are
deliberately NOT changing.]
 
**Implementation Plan**
 
1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]
[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]
 
**Toolkit**
 
- Tool:
- What it produced:
- Difficulties:
- What changed:
---
 
## Testing Strategy
 
[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]
 
**Toolkit**
 
- Tool:
- What it produced:
- Difficulties:
- What changed:
---
 
## Implementation Notes
 
[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]
 
**2026-11-11**
 
issue-scout accepted #4613 on all six checks. I claimed it; lgritz replied "@stevwya77 Sounds good!".
 
**2026-11-13 – 2026-11-14**
 
Wrote `repro.py` and a CI matrix (3 OS × 4 wheels). Found the leak is 5 bytes per pixel value per frame. Switched to measuring `phys_footprint` on macOS after RSS stopped showing growth. Bracketed the fix between 3.0.5.0 and 3.0.6.0 and matched it to #4685. Wrote `probe.py`, which found `get_pixels(oiio.UNKNOWN)` returning `None` or crashing. repro-check accepted my claim and repro draft and listed fixes.
 
**2026-11-18**
 
The voice guard stopped three drafts (02:19, 02:28, 04:21) and I fixed each. Wrong turn: my crash snippet reproduced 0 of 10 times in my own terminal; opening the file in a fresh process, as `probe.py` does, gave 10 of 10, so I changed the #5698 steps. I re-ran `probe.py`: under guard-malloc it now showed SIGSEGV instead of the Nov 14 SIGABRT, so I updated the evidence and cut the outdated paragraph. Filed #5698 and posted my report-back on #4613. lgritz closed #4613 ("Fixed by #4685") and labeled #5698 `bug`, `python`, `good first issue`. The course chat approved #5698 as my contribution.
 
**Toolkit**
 
- Tool:
- What it produced:
- Difficulties:
- What changed:
---
 
## Pull Request
 
**PR Link:** [GitHub PR URL when opened]
 
**PR Description:** [what the PR says; much of the content above
adapts into it]
 
**Maintainer Feedback:**
 
- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]
**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]
 
**Toolkit**
 
- Tool:
- What it produced:
- Difficulties:
- What changed:
---
 
## Learnings and Reflections
 
**Technical Skills Gained**
 
[What you learned technically, named concretely.]
 
**Challenges Overcome**
 
[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]
 
**What I'd Do Differently Next Time**
 
[Reflection on your process.]
 
---
 
## Resources Used
 
[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]
 
- OpenImageIO CONTRIBUTING.md, bug report template, and AI policy: https://github.com/AcademySoftwareFoundation/OpenImageIO/blob/main/docs/dev/AI_Policy.md
- Issue #4613 thread, including lgritz's triage note: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613
- PR #4685 (fix(python): fix handle leak): https://github.com/AcademySoftwareFoundation/OpenImageIO/pull/4685
