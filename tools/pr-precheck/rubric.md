# Rubric: is this pull request ready to submit?

## Checks

Package parts are named as they appear in a bundle; in live mode, Plan
context is `plan.md` (its `## Deviations` included), Title and
Description are `pr_draft.md`, Diff is `git diff main...HEAD`, Commits
is `git log main..HEAD`, Test evidence is `test_evidence.md`, and Repo
facts are `.github/PULL_REQUEST_TEMPLATE.md` plus
`docs/CONTRIBUTING.md`.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The Diff's changed files and hunks, read against the plan in Plan context (its steps, its Files line, its not-in-scope statements, and any recorded deviations) | passes only if every change in the Diff is something the plan or a recorded deviation names, and every plan step appears in the Diff or its absence is disclosed in the Description; fails if the Diff silently does more than the plan (extra files, extra behavior, anything the plan lists as not in scope) or silently skips a plan step | required |
| description-matches-diff | Every claim in the Title, Description, and Commits about what changed, what was tested, and what the change fixes, read against the Diff and the Test evidence | passes unless a claim would mislead a reviewer about what the change does, what it touches, or how it was tested; fails if a claim is contradicted by the Diff or Test evidence, or names something the package should show and doesn't (for example "only touches X" when the Diff touches Y, "adds a regression test" with no test in the Diff, "implements the plan exactly" when the Diff leaves out a plan step). Loose summaries are not mismatches: "one function touched" when the Diff also adds a helper that function calls, or a slightly off count of test cases, still passes. Confirmations of routine checks ("code formatted", "clippy clean", "go generate produces no diff") are taken at face value unless the Diff or Test evidence contradicts them. Confident wording is fine when the evidence supports it | required |
| test-evidence | The Test evidence, read against the plan's test plan and the Repro evidence in Plan context | passes only if the Test evidence shows real output from after the change (the repro re-run, or a check through the real code) whose result observably differs from the failing Repro evidence in the way the plan expected; fails on claims with no output ("verified it works", "tests pass"), output that does not exercise the changed behavior, or output from before the change only. A planned test that was not run passes only if the Description says so and why | required |
| repo-checks | The Test evidence and the Diff, read against the test and check asks in Repo facts (the template's listed checks, "tests with behavior changes", lint or format rules) | passes only if every check the repo asks for is reported by name or command with its result, and a test was added or updated when the repo asks for tests with behavior changes. A named result line counts as reported ("`cargo test -p nu-protocol` passes (312 tests)", "fmt and clippy clean"); pasted output is not required for the repo's suite or linters (the behavior proof is test-evidence's job). Fails if a check the repo asks for is never mentioned, or is reported only as a vague "all tests pass" with no command or suite named. A check that fails or could not run passes when the result and the reason are reported honestly. Passes if Repo facts state no test or check ask | required |
| reviewable-diff | Each hunk in the Diff, read against the plan's change | passes only if every hunk serves the planned change; fails on debug output or print statements, commented-out code, stray or generated files, whitespace or formatting churn in untouched code, or unrelated edits; an unrelated hunk passes only if a recorded deviation explains why the fix needs it | required |
| template-and-policy | The Title and Description, read against the PR template asks and the contribution policy in Repo facts (required sections or checkboxes, title format, issue reference, AI-use disclosure) | passes only if every explicit ask the PR text can satisfy is met, or the Description says why it is not; fails if a stated ask is ignored (a required section missing, a checklist item the Diff or Test evidence contradicts, such as "tests added" with no test in the Diff, no issue reference where one is required, no AI disclosure where the policy requires one); asks outside the PR text (signing a CLA, a vouch flow) pass unless the package shows they were not met; passes if Repo facts state no template or policy ask | required |
| title-describes-change | The Title, read against the Diff | passes if a reviewer could tell from the Title alone what the Diff changes; fails on vague titles such as "fix bug" or "update file" | preferred |

The Candidate PR is the work being graded. The plan is the standard it
is read against; never grade the plan itself.

A shortfall the Description discloses honestly (a skipped step, a
check that failed, a test that could not run, with the reason) is not
silent and does not fail plan-fidelity, test-evidence, or repo-checks
on its own. "Less than everything" is not "hold"; "less than everything
and not saying so" is.

## Verdict rule

Accept (ready to submit) only if every `required` check passes. If any
required check is graded `fail` or `unclear`, reject (hold).

Grade a check `unclear` only when the part its Evidence column names is
absent from the package (for example, no Test evidence section at
all). When Repo facts state no rule for a check, that check passes; it
is not `unclear`. Record in the evidence line whether a hold came from
`fail` or `unclear`, so missing data is distinguishable from a real
failure.

`preferred` checks never change the verdict.
