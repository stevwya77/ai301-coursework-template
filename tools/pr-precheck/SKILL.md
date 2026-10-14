---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence), read against the plan
it claims to implement and the issue that plan belongs to. Answer only
that question, grade only one package per run, and never answer from
gut feel. You answer by executing the student-authored procedure in
`procedure.md`, which applies the rubric in `rubric.md` to evidence
gathered per `references/evidence-guide.md`.

## Inputs and modes

Decide your mode before reading anything else. If you were given a
package bundle (a markdown file from `eval/packages/`), you are in
eval mode. Otherwise you are in live mode.

**Live mode**: the student's own PR, checked before it is opened.
Gather exactly these inputs, and nothing else:

- **Plan**: `plan.md` in the working copy, including anything under
  `## Deviations`. This is what the PR claims to implement. A deviation
  recorded there is part of the plan and gets graded like the rest of
  it; a change that appears only in the diff, with no deviation note,
  is not part of the plan.
- **Diff**: run `git diff main...HEAD` (three dots) from the working
  copy. This is every committed change the branch makes relative to
  main. Use `git log main..HEAD` for the branch's commits. Uncommitted
  or untracked changes are not part of the PR; do not read or grade
  them.
- **Title and description**: `pr_draft.md` in the working copy. Line 1
  is the PR title; everything after it is the PR description.
- **Test evidence**: `test_evidence.md` in the working copy.
- **Issue**: the issue URL the student gives. Read the issue and its
  full comment thread with `gh` or the web.
- **Repo standards**: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` from the scoped repo.

A house-chain student reads the house plan and the house repro pack in
place of their own; the same checks grade the same things there.

Read the drafts the way a maintainer reading the opened PR would: the
package is what the PR title, description, commits, and diff contain,
read against the plan, the issue, and the repo's stated standards. Do
not treat other files in the working directory as part of the PR.

If `plan.md` or `pr_draft.md` is missing or empty, or the diff is
empty, stop without grading and name exactly which input is missing.
Missing or incomplete test evidence is not a reason to stop: grade it
as the rubric directs.

**Eval mode**: the bundle is the whole world. Use only the bundle text
as evidence. Fetch nothing, run no git or `gh` commands, and read no
files outside this skill's directory. Eval mode always grades a
complete package: run every check in the rubric and apply the full
verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names the repo the student's issue and PR must live in, and
the house rules of that environment. Apply those house rules wherever
they change how evidence is read; a house rule outranks a general
expectation.

Refuse to grade a package whose issue or PR is outside the scoped
repo, and say which repo was expected. If the `Repo:` line in
`scope.md` still carries an unfilled placeholder, stop without grading
and tell the student to fill the `Repo:` line in `scope.md` with their
section's Path Review repo. Never guess a scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, with wrong and right examples. Hold the
outgoing PR text, the title and the description from `pr_draft.md`,
against those rules. In the summary, report every rule the draft
breaks: quote the rule, then quote the line that breaks it.

The voice guide never changes the verdict on its own. It changes the
verdict only if `rubric.md` has a check that reads it. Voice is
personal; the universal communication-quality checks live in the
rubric.

In eval mode, ignore `voice-guide.md` entirely: voice carries no gold
labels.

## Component reads

The components connect like this:

1. `rubric.md` defines the checks (each with the evidence to gather,
   its pass condition, and its weight) and the verdict rule.
   `required` checks gate the verdict; `preferred` checks never change
   it.
2. `references/evidence-guide.md` is the rubric's map: where each
   evidence family lives in a PR package, in a bundle and in live
   mode, and what good evidence looks like.
3. `procedure.md` is the operating procedure: read order, evidence
   gathering, check execution, and verdict assembly. Execute it as
   written, exactly, the way an executor follows a rubric.

Where the procedure is silent on a step, do not invent one. Report the
gap in your summary so the student can fix it; a procedure gap is
feedback.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop without grading and say which file is empty. This tool
cannot grade without a rubric and a procedure, by design: a tool that
invents checks at runtime produces verdicts that look like judgment
and are noise. Instruction comments inside those files do not count as
content.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict, no "accept with reservations", and
no score. Reservations belong in a check's evidence line, not in the
verdict.

Before the JSON block, you may show a short readable summary: one line
per check, plus, in live mode, any voice-guide notes and any procedure
gaps. Then end your reply with this fenced JSON block, valid and last,
with nothing after it. The eval harness parses the last fenced JSON
block in your output.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be
  ready, and a long, confident one can be hiding drift. Read every
  check against the artifact itself (the diff, the commits, the test
  output) compared with the plan, the issue, and the repo's stated
  standards, never against formatting or tone.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict
  rule directs. If the rule does not say, treat `unclear` as `fail`: a
  PR you cannot verify from the package is a PR that is not ready to
  submit.
