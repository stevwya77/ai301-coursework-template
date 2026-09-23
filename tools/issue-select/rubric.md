# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: the `last 5 default-branch commits` list and the `maintainer first-response sample` | the most recent non-bot commit is within 60 days of the capture date, AND at least one sampled issue drew an owner/member/collaborator reply within 60 days. When no issue in the response sample has any maintainer reply at all, the sample carries no information and the commit condition alone decides | required |
| unclaimed | Repo facts: `this issue: assignees:` and `linked PRs:` with state, plus the Comments section | passes only if all three hold: `assignees:` is none; no linked PR is in the open state (a closed or merged PR is an abandoned or finished attempt, not a live claim); and the thread holds no live claim. A claim comment ("I'll take this", "can I work on this", "working on this", or a `/assign` command) counts as live unless a human maintainer (owner, member, or collaborator) commented later in the thread with no sign the claimer is still on it. A bot comment never clears a claim | required |
| newcomer-scope | The issue body, its labels and author association, and the Comments section | fails if any of these hold: the issue explicitly presents itself as an umbrella or tracking issue, meaning a list of separate work items each meant to become its own issue; the comment thread shows the approach is still being debated with no maintainer decision; a maintainer states the fix touches core internals; the issue is a usage or support question rather than a request for a change; or the issue is a feature request with no maintainer endorsement at all, meaning it has no comment from an owner, member, or collaborator AND no triage label AND was not opened by one of them. Otherwise passes. A detailed implementation plan, a list of possible causes within a single change, a terse body, a missing reproduction, or a bare checklist is not a scope failure | required |
| ai-policy | Repo facts: the `contribution policy` line | fails only if the stated policy bans AI-generated or AI-assisted contributions outright. Conditions such as disclosure, personal understanding, testing, or human review of AI output pass. A repo with no stated policy passes | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only if every `required` check passes. If any required check is
graded `fail` or `unclear`, reject. `preferred` checks never change the
verdict; they only rank accepted issues in live mode.
