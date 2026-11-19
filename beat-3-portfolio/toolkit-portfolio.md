# Toolkit Portfolio
 
Path: `beat-3-portfolio/toolkit-portfolio.md`
 
This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.
 
**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.
 
**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.
 
**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.
 
**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.
 
**Before you finish**, replace every `[bracketed prompt]` with your own words, then delete
the unused `[Tool name]` entry stub, its copy-me sentence, and the prompt sentences under
each header. A leftover bracketed prompt reads as unfilled.
 
**Student:** Stephanie Wyatt
 
## Featured Tools
 
I chose these three because they carried my real contribution from start to finish: issue-scout picked OpenImageIO #4613, repro-check graded the reproduction I built for it, and my voice guard checked everything I posted upstream, including the new bug I filed as #5698. All three changed because of something that went wrong while I used them, and each one caught a real problem in my own work.
 
**issue-scout**
 
**What it is, and the judgment hole I wrote:** issue-scout turns my fit profile into GitHub searches, ranks the results, and grades the top five against my rubric. The judgment is mine: the rubric's checks (maintainer-alive, unclaimed, newcomer-scope, ai-policy, and later repo-in-use and accepts-outside-prs) and the search strategy that decides what gets graded.
 
**How it evolved, dated:**
- 2026-09-22: built as issue-select in Unit 1. Its first eval scored 15/20 against the gold labels, below the 18/20 bar. I revised the rubric, including an unclaimed check that tells real human claims from bots and abandoned ones, and the next run scored 19/20.
- 2026-10-30: upgraded into issue-scout in Unit 5, with my own search strategy. Real scans showed the rubric passing repos that ban AI-written code, refuse outside pull requests, or barely have users. Ladybird's CONTRIBUTING.md says "We do not accept pull requests from non-maintainers," and my ai-policy check only looked for AI bans, so a clean Ladybird issue would have passed.
- 2026-10-30 to 2026-11-11: added repo-in-use and accepts-outside-prs, tightened unclaimed and newcomer-scope, and ranked my priority repos first. After the patch, a scan came back 0 accepted and 5 rejected, all correctly on repo-in-use (2, 13, 0, 0 and 0 stars).
- 2026-11-11: it accepted OpenImageIO #4613 on all six checks. I claimed it, and the maintainer replied "@stevwya77 Sounds good!"
**Evidence:**
 
The two rows I added (from my rubric, trimmed):
 
```text
| repo-in-use | ... fails if the repo has fewer than 20 stars or was created less than 90 days before the capture date ... | required |
| accepts-outside-prs | ... fails if the stated policy says the repo does not accept pull requests from outside contributors ... | required |
```
 
The verdict on my real contribution, 2026-11-11:
 
```text
accept verdict on OpenImageIO #4613 with all six checks passing (2,391 stars,
maintainer reply within ~1.5 hours on a recent issue, no claims or PRs, a triaged
single bug, outside PRs welcome, AI allowed with disclosure)
```
 
**repro-check**
 
**What it is, and the judgment hole I wrote:** repro-check grades a claim comment and a reproduction report before I post them, and says whether someone else could follow and trust them. The judgment is mine: the rubric's checks (environment, steps, behavior shown, honesty, communication, and a preferred control run) and the voice guide it reads.
 
**How it evolved, dated:**
- 2026-09-30: built in Unit 2. I first made the environment check require any setting people in the thread linked to the bug. That rejected pkg-01, a report the gold set marks as accept, over commenters' version remarks. I changed it so only the issue or a maintainer can make a setting required. pkg-01 passed again, and the final eval scored 20/20.
- 2026-11-14: in Unit 6, I gave it the scope for real repos and ran it on my real OpenImageIO #4613 claim and reproduction draft. It accepted the draft but flagged that Linux results were claimed without output shown, and that one explanation was written as fact.
- 2026-11-18: my report-back on #4613 kept the measured results and left those explanations out. The maintainer closed the issue: "Thanks for checking. I will close this issue, then. Fixed by #4685."
**Evidence:**
 
My environment rule after the Unit 2 change:
 
```text
passes only if the record names the OS, the tool's version, and how the tool was installed,
plus any setting the issue, a maintainer comment, or the bug reports: line says matters
```
 
Its read-out on my real #4613 draft, 2026-11-14 (trimmed):
 
```text
Verdict: accept. All five required checks pass, and so does the preferred control-run check.
 
Things to fix before posting (none of them change the verdict):
1. Linux has no output in the report.
2. The 3.2.2.0 explanation is written as fact.
```
 
**voice guard**
 
**What it is, and the judgment hole I wrote:** The voice guard checks anything I'm about to post upstream against my voice guide and stops the post if a rule is broken. The checker came with Unit 6. The judgment is mine: the voice guide, including the Upstream section I wrote with rules for claims, questions and pings.
 
**How it evolved, dated:**
- 2026-11-14: installed in Unit 6, with my Upstream section added to the voice guide.
- 2026-11-18, 02:19: it stopped my #5698 bug report draft. The line "happy to work on a fix" offered work without a first step or a report-back date, which breaks my claim rule. I cut it.
- 2026-11-18, 02:28: it stopped the next draft for a root-cause guess I hadn't traced. I deleted that sentence.
- 2026-11-18, 04:21: it stopped my #4613 report-back draft for subjective environment words like "latest". After I fixed it, the draft passed, and I posted both pieces.
**Evidence:**
 
From my guard log, 2026-11-18:
 
```text
2026-11-18 02:19 [manual] stop: rule broken: "Never offer or claim work without naming, in the same
comment, a specific first step I can start today and the date I will report back. A claim never
promises a fix, ..." | target: issue.md
```
 
Before and after, as filed in #5698:
 
```text
Before: Found while writing a reproduction for #4613; happy to work on a fix.
After:  Found while writing a reproduction for #4613.
```
 
The second catch, 02:28, and the line it removed:
 
```text
stop: rule broken: "Unless I have traced the exact line of code and am opening a PR,
I will not speculate on the root cause." | target: issue-ready.md
 
Deleted: That pattern suggests an out-of-bounds access rather than a clean "unsupported format" failure.
```
 
## Inventory
 
- plan-check (Unit 3): grades an implementation plan before coding. I changed its honesty check to judge evidence instead of confident wording, which moved its eval from 18/20 to 19/20. I used it on my Path Review #69 plan.
- pr-precheck (Unit 4): grades a pull request before opening it. I loosened its description-matches-diff check so a loose summary isn't treated as a mismatch, which moved its eval from 17/20 to 20/20. I used it on Path Review PR #207.
- Unit 1 eval harness (`run_eval.py`): scores a tool against 20 gold-labeled packages. It's still the only regression check for my rubric changes.
- Repo memory (`CLAUDE.local.md`, Unit 6): not set up yet. I couldn't confirm a memory file for my OpenImageIO clone.
- ship-gate (Unit 7) and PR-monitor (Unit 8): not run yet. They need a fix branch and a pull request, which will come from my #5698 fix.
## The Arc
 
**Beat 1**
Date: 2026-09-22
Friction: issue-select's first eval scored 15/20, below the 18/20 bar.
Change: I revised the rubric, including an unclaimed check that tells real human claims from bots and abandoned ones.
Result: The next run scored 19/20, and the tool accepted Path Review #69, which I went on to reproduce, plan and fix in PR #207.

Date: 2026-09-30
Friction: repro-check rejected pkg-01, a report the gold set marks as accept, because my environment check counted settings that commenters mentioned.
Change: Only the issue or a maintainer can now make a setting required.
Result: pkg-01 passed again, and the final eval scored 20/20 with all wrong-target reports still rejected.
 
Date: 2026-10-13
Friction: pr-precheck scored 17/20 because it read "one function touched" literally and rejected a correct PR that also added a helper.
Change: I loosened the description-matches-diff check so it only fails a description that would mislead a reviewer.
Result: It scored 20/20, and I used it before opening Path Review PR #207.
 
**Beat 2**
Date: 2026-10-30 to 2026-11-11
Friction: Real scans with issue-scout passed repos that ban AI code, refuse outside pull requests, or barely have users.
Change: I added the repo-in-use and accepts-outside-prs checks and tightened unclaimed and newcomer-scope.
Result: The next scan rejected all five weak repos correctly, and on 2026-11-11 the scout accepted OpenImageIO #4613, where the maintainer approved my claim.
 

Date: 2026-11-14
Friction: My first real reproduction report claimed Linux results it didn't show and stated a cause as fact.
Change: I ran repro-check on it, and for the post I kept only measured results.
Result: On 2026-11-18 the maintainer read my report-back and closed #4613 as fixed by #4685.

 
Date: 2026-11-18
Friction: My bug report draft for the new get_pixels bug offered a fix without a plan and guessed at the cause.
Change: The voice guard stopped both lines, and I cut them before filing.
Result: I filed #5698, and the maintainer typed it as a Bug and labeled it bug, python and good first issue.
 
## Limitations and What's Next
 
My toolkit is strongest where it had gold labels to train against. issue-select, repro-check, plan-check and pr-precheck were all tuned on 20-package eval sets, but the tools I used on real projects have no gold labels, so the only regression check is re-running my Unit 1 harness. The scout's rubric held up in real scans, but its ranking did not: putting my priority repos first keeps filling the top five with unlabelled issues, and my latest scan graded five issues with none usable. repro-check is also looser than my own voice guide about guessing causes, so the voice guard ended up catching what repro-check let through. Next, I'll add a label-based drop rule and require good-first-issue or help-wanted labels on the scout's priority queries, bring repro-check's honesty rule in line with my voice guide, and run ship-gate and PR-monitor on my fix for #5698.
 
**Tool limits.** One line per featured tool, in the same order as the entries above, naming
where that tool still stops or what it still gets wrong. A small honest limit is full credit;
"none really" is not a limit. If a tool genuinely has none you can name, write `none` after
its label rather than deleting the line.
 
issue-scout: its ranking puts unlabelled priority-repo issues first, so my latest scan graded five issues and none were usable.
repro-check: it passes cause-guesses worded as guesses, even though my own voice guide says not to guess causes at all.
voice guard: it only checks what I post through Claude Code or a draft I remember to run with --draft, so a post typed straight into the browser skips it.
