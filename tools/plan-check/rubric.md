# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis | The cause stated in the Candidate plan, read against the Repro evidence (including any control run or debug output) and maintainer statements in Thread highlights | passes only if the stated cause explains the behavior the Repro evidence shows and nothing in the evidence contradicts it; fails if a control run or debug output rules out the blamed component, or if the plan patches the symptom while the evidence points at a different cause | required |
| scope | The Candidate plan's change, in-scope and out-of-scope statements and the files it names, read against the behavior reported in the Issue | passes only if every change named is needed to fix the reported behavior; fails if the plan adds refactors, renames, migrations, dependency upgrades, or a redesign the issue doesn't require | required |
| executable | The files, functions and approach named in the Candidate plan | passes only if someone new to the issue could start the first edit without asking the author anything: the plan names where the change goes (file or function) and what the change is | required |
| test-plan | The Candidate plan's test section, read against the steps and output in the Repro evidence | passes only if the plan re-runs the repro (or a check through the real code) and names an observable result after the fix that differs from the failing output; "verify it works" or "run the tests" with no expected result fails | required |
| honesty | Certainty language in the Candidate plan and Candidate plan comment ("the cause is", "confirmed", "this fixes"), read against the Repro evidence and Thread highlights | passes only if every certainty claim is backed by something the Repro evidence or Thread highlights shows; confident wording is fine when the evidence supports it; fails only claims the evidence doesn't show and the plan doesn't name as an unknown | required |
| thread-convention | The Candidate plan comment, read against maintainer (OWNER/MEMBER/COLLABORATOR) direction in Thread highlights and the contribution policy line under Repo facts | passes only if the comment follows any explicit maintainer direction in the thread, or says why it doesn't, and meets any disclosure or format rule the policy states; passes if the thread and policy set no such rule | required |
The Candidate plan comment is the work being graded; never treat it as a competing plan on the issue.

## Verdict rule

Accept (ready) only if every `required` check passes. If any required
check is graded `fail` or `unclear`, reject (hold).

Grade a check `unclear` only when the part its Evidence column names is
absent from the package (for example, no Candidate plan comment at all).
Record in the note whether a hold came from `fail` or `unclear`, so missing
data is distinguishable from a real failure.

`preferred` checks never change the verdict; they only rank accepted
plans.
