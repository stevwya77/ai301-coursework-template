# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

**Where it lives:**

| Signal | Live (GitHub or the draft) | In the eval bundle |
|---|---|---|
| The environment the report records | the environment line in the draft repro comment | the environment line of the Candidate repro report (usually starts "Environment:") |
| The environment the issue targets | the version and OS in the issue's opening post, plus any maintainer comment that narrows it ("only in Release builds") | the version and OS in the Issue section, plus Thread highlights |
| What the repo asks a report to record | the bug report form (Issues → New issue), or the files in `.github/ISSUE_TEMPLATE/` | the "bug reports:" line under Repo facts |

**What good looks like:** 

the report names the OS, the tool's version and how it
was installed, plus any setting the issue or template says matters (driver,
build type, browser). Wherever these differ from what the issue targets, the
report says so. An unmentioned difference fails.

## Steps

**Where it lives:**

| Signal | Live (GitHub or the draft) | In the eval bundle |
|---|---|---|
| The steps the report gives | the commands, code blocks or numbered actions in the draft repro comment, plus any input files they create or describe | the same parts of the Candidate repro report |
| The trigger the issue gives | the steps, command or code example in the issue's opening post | the steps, command or code example in the Issue section |
| Corrections to the trigger | maintainer comments that change what it takes to trigger the bug (a flag, a build type, an input) | Thread highlights, especially comments marked OWNER or MEMBER |
| What the repo asks steps to include | the bug report form (Issues → New issue) or `.github/ISSUE_TEMPLATE/` | the "bug reports:" line under Repo facts |

**What good looks like:** Pass if someone with the recorded environment could
(1) copy or recreate every input from what the report or the issue shows, with
nothing private or unshared; (2) run the command and inputs the issue, or a
maintainer in the thread, says trigger the bug, with any change from them
stated plainly; and (3) never have to guess a command, file, flag or setting.
How the steps are formatted doesn't matter.

## Behavior shown

**Where it lives:**

| Signal | Live (GitHub or the draft) | In the eval bundle |
|---|---|---|
| What the report's output shows | the output, logs or error messages pasted into the draft repro comment, including any control run | the output blocks in the Candidate repro report, including any control run |
| What the issue says happens | the error, output or behavior described in the issue's opening post | the error text, exit code or output described in the Issue section |
| Corrections to the behavior | maintainer comments that narrow or clarify the symptom | Thread highlights, especially comments marked OWNER or MEMBER |

**What good looks like:** Pass if the pasted output shows the same symptom the
issue (or a maintainer in the thread) describes, judged by the details that
define it: the error message, the exit code, whether it crashed, or the wrong
output. Machine-specific details like timestamps and file paths may differ.
Output that only shows the program ran or was set up does not pass. Or, if the
report says it could not reproduce, the output shows what actually happened
when the steps ran.

## Honesty

**Where it lives:**

| Signal | Live (GitHub or the draft) | In the eval bundle |
|---|---|---|
| The claims | sentences in the draft claim and repro comments that say what happened or why: "reproduced", "confirmed", "the cause is", "also affects…" | the same sentences in the Candidate claim comment and Candidate repro report, especially summary, Result, Actual and Conclusion lines |
| The backing | the output pasted into the draft repro comment | the output blocks in the Candidate repro report |
| What the thread already established | comments in the issue thread, such as a maintainer who couldn't reproduce, or versions where it was confirmed | Thread highlights and the Issue section |

**What good looks like:** Pass if every claim is backed by the output shown in
the report and reaches no further than what the recorded data actually proves;
the report says plainly whether it reproduced the bug, even when it didn't; and
any theory about the cause is worded as a guess ("seems", "may", "likely"), not
stated as fact. Words like "thorough", "100%" or "confirmed" are not
substitutes for the output shown.

## Comms

**Where it lives:**

| Signal | Live (GitHub or the draft) | In the eval bundle |
|---|---|---|
| The message to maintainers | the text of the draft claim comment meant for the issue thread | the Candidate claim comment |
| Repo rules on format and AI | `CONTRIBUTING.md`, `.github/ISSUE_TEMPLATE/`, or any repo-wide AI policy | the lines detailing contribution rules or AI policies under Repo facts |
| Boilerplate and placeholders | unfilled `[Insert here]` markers or generic filler in the draft comment | the text of the Candidate claim comment |

**What good looks like:** 
Pass if the claim comment states the exact outcome for this specific issue without generic filler ("I have extensively reproduced this"), robotic pleasantries, or unfilled template placeholders. If the repo rules require an explicit AI-use disclosure or a specific template format, the comment includes them exactly as requested; if it doesn't, the comment does not invent a template the repo didn't ask for.
