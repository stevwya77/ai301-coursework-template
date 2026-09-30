# Rubric: is this reproduction package ready to post?
 
<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.
A filled rubric must contain:
1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).
2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.
Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->
 
## Checks
 
| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment | The repro report's environment record (the `Environment:` line of the Candidate repro report), read against the version and OS the Issue section and Thread highlights target, and the `bug reports:` line under Repo facts | passes only if the record names the OS, the tool's version, and how the tool was installed, plus any setting the issue, a maintainer comment, or the `bug reports:` line says matters (driver, build type, browser, dependency version). Wherever the recorded tool version differs from the version the issue or a maintainer targets, the report says so in words. A difference in OS, install method, browser, or other setting must be stated only when the Issue section itself or a maintainer (OWNER/MEMBER/COLLABORATOR) comment ties the bug to that setting (e.g. "Windows only", "Release builds", "non-English browser language"); an untied difference does not fail, and a cause suggested only by non-maintainer commenters (e.g. "may be the multidict upgrade") does not create a requirement. A missing record, a missing item, or an unmentioned difference that must be stated fails. If the issue names no version, any recorded version passes | required |
| steps | The commands, code blocks and inputs in the Candidate repro report, read against the trigger in the Issue section and any maintainer (OWNER/MEMBER/COLLABORATOR) correction to it in Thread highlights | judged on the attempt whose output the report shows. Passes only if all three hold: (1) every input that attempt uses (file, script, config, payload) is either shown in the report or the Issue section, or described precisely enough that any faithful recreation from the report plus the Issue section would trigger the same behavior (e.g. "the issue's two `format` calls with the ranges it gives", "an env.yml with a valid dependencies list plus a `category:` section"); nothing private, elided, or left as a placeholder, and no detail that affects the outcome left to guess; (2) the steps run the trigger the issue or a maintainer gives, or, when the issue describes a condition rather than a command, a concrete attempt to create that condition; any change from the issue's trigger (an added flag, a local file instead of a URL, an offline mode) is stated plainly; (3) someone with the recorded environment would never have to guess a command, flag or setting for that attempt. Extra variants the report only describes in prose do not fail this check. How the steps are formatted or numbered does not matter | required |
| behavior-shown | The output excerpts in the Candidate repro report (including any control run), read against the symptom the Issue section describes and any maintainer correction to it in Thread highlights | passes if the output shows the issue's own symptom, judged by its defining detail: the error message, exit code, crash, or the specific wrong output. Timestamps, paths and hostnames may differ. Also passes if the report says it could not reproduce and the output shows what actually happened. Fails if the output shows a different or adjacent symptom (another error, another code path, another component), shows only installation or setup succeeding, or if the symptom is described in prose but not visible in any output | required |
| honesty | The outcome sentences in the Candidate claim comment and Candidate repro report ("reproduced", "confirmed", "could not reproduce", "the cause is", "also affects…"), read against the report's output excerpts and what Thread highlights already established | applies only to claims about the bug (whether it reproduced, what the symptom is, where it occurs, what causes it); statements about the contributor's own process, understanding, or AI use are judged under comms, not here. Passes only if all hold: every stated outcome is visible in the output shown, where saying the output matches the issue's reported behavior ("still present on X", "unchanged", "matches the report") counts as backed when the shown output has the issue's symptom; prose details that support the shown result without changing it (a repeat count, a described variant with the same outcome) are allowed, and so is a prose-only side observation that does not bear on whether the reported symptom reproduced and agrees with what a maintainer already stated (e.g. "without `--replace` the numbers are correct", after the owner said `--replace` is required); the report's main outcome must still be visible in output; no claim reaches beyond the recorded environment (e.g. "affects all versions" from one run); the report says plainly whether it reproduced the bug, in any wording; any explanation of the cause is worded as a guess ("may", "likely", "want to check") unless the thread already established it; and nothing contradicts a fact the thread established (a maintainer could not reproduce, a confirmed or excluded version, a stated trigger) without saying so. Open suggestions in the thread about what the correct behavior should be ("maybe the message shouldn't show at all") are not established facts; restating the issue's own expected behavior does not contradict them. An evidenced cannot-reproduce passes; a confident "reproduced" whose output shows a different symptom fails. Intensifiers ("thorough", "100%", "definitely") never count as backing | required |
| comms | The Candidate claim comment, read against the `contribution policy` line and any format rules under Repo facts | passes only if all hold: the comment names something specific to this issue (the symptom, command, version, or component involved); it contains no unfilled placeholders (`[Insert …]`, `<your name>`, `TODO`) and no filler that would fit any issue ("I have extensively investigated this"); if the policy requires an AI-use disclosure or a specific format, the comment includes it as the policy words it; if the policy requires none, the comment does not invent a template the repo did not ask for. A repo with no stated policy requires no disclosure | required |
| control-run | The output excerpts in the Candidate repro report | passes if the report includes a second run that differs from the failing run in one stated way and does not show the symptom | preferred |
 
The Candidate claim comment is the work being graded; never treat it as a
competing claim on the issue.
 
## Verdict rule
 
<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
 
Accept (ready) only if every `required` check passes. If any required
check is graded `fail` or `unclear`, reject (hold).
 
Grade a check `unclear` only when the part its Evidence column names is
absent from the package (for example, no Candidate repro report at all).
Record in the note whether a hold came from `fail` or `unclear`, so missing
data is distinguishable from a real failure.
 
`preferred` checks never change the verdict; they only rank accepted
packages.
 
