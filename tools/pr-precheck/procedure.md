# Procedure: how this tool grades a PR package

## Read order

Read every part before grading any check. The plan is read before the
diff, and the description is read last, on purpose: the plan is the
yardstick the diff is measured against, so it has to be in hand first,
and the description is the author's story about the diff, so reading
it before the diff anchors you to that story instead of to what the
diff actually does.

1. **Repo facts.** Note the pull-requests line and the contribution
   policy line word for word. List every explicit ask separately:
   required template sections or checkboxes, title format, issue
   reference, AI-use disclosure, tests with behavior changes, named
   checks to run (test, lint, typecheck, format). If a line says "no
   PR template" or "no stated AI policy", note that no rule applies
   there.
2. **Issue.** Note the reported behavior in one sentence (what
   happens, and what should happen instead).
3. **Thread highlights.** Note every comment from an OWNER, MEMBER or
   COLLABORATOR that gives direction about the fix or the PR (a
   requested approach, a "don't do X", a request for a test). Comments
   marked NONE or CONTRIBUTOR are context, not direction.
4. **Plan context.** Write down, as separate lists:
   - the plan's steps, one line each;
   - the files and components the plan names;
   - everything the plan says is not in scope;
   - the test plan and the expected result it names;
   - the failing output in the Repro evidence;
   - any recorded deviations (under `## Deviations` in live mode), with
     what changed and why.
5. **Diff.** Read it hunk by hunk. For each hunk, note the file and,
   in a few words, what the hunk changes.
6. **Commits.** Note each commit message.
7. **Test evidence.** Note each command shown, whether it ran before
   or after the change, and its output.
8. **Title and Description.** Read them once in full.

In live mode the same parts are: `.github/PULL_REQUEST_TEMPLATE.md`
and `docs/CONTRIBUTING.md` (1), the issue body (2), the issue's
comments (3), `plan.md` with its `## Deviations` section and the
student's posted repro comment, or the house repro pack for a house
issue (4), `git diff main...HEAD` (5), `git log main..HEAD` (6),
`test_evidence.md` (7), and `pr_draft.md`, line 1 as the title and the
rest as the description (8).

## Evidence gathering

For each check, build the pairing below and record it as short
quotes, side by side.

1. **plan-fidelity:** two lists, side by side. Left: every hunk from
   read order step 5. Right: every plan step, named file, and recorded
   deviation from step 4. Draw a line from each hunk to the plan item
   it carries out. Mark any hunk with no line as "extra", and any plan
   step with no hunk as "missing". For each extra or missing item,
   copy any Description sentence that discloses it. Also note any hunk
   that touches something the plan lists as not in scope.
2. **description-matches-diff:** every claim in the Title, the
   Description, and the commit messages, one per line. Beside each,
   write the hunk or the Test evidence output that backs it,
   "contradicted by ..." with the line that contradicts it, or
   "nothing backs this". Claims to collect: what changed ("removes",
   "adds", "only touches"), what was tested ("tests added",
   "verified"), what is fixed ("fixes #N", "no longer errors"), and
   fidelity claims ("implements the plan exactly", "no other
   changes"). Mark confirmations of routine checks ("code formatted",
   "lint clean", "no cheatsheet diff") as "routine" instead of
   hunting for output.
3. **test-evidence:** left, the plan's test plan and its expected
   result, plus the failing Repro evidence output. Right, the Test
   evidence output from after the change. For each planned test, note
   "ran, output shown", "claimed, no output", or "not run". For any
   test not run, copy any Description sentence that says so and why.
4. **repo-checks:** left, every test or check ask listed in read order
   step 1. Right, the line in Test evidence or the Description that
   reports it by name or command with a result (pasted output or a
   result line such as "`cargo test -p ignore` passes (214 tests)"),
   or "not mentioned", or "vague" for an unnamed "all tests pass".
   Where the repo asks for tests with behavior changes, add any test
   file or test function the Diff adds or changes. For a check that
   failed or could not run, copy the reason given.
5. **reviewable-diff:** go through every hunk and note any that are:
   debug output or print statements, commented-out code, stray or
   generated files, whitespace or formatting changes to lines the fix
   does not need, or edits unrelated to the plan's change. For each,
   copy any deviation note that explains it.
6. **template-and-policy:** left, every ask from read order step 1.
   Right, the part of the Title or Description that meets it, or
   "missing". For each ticked checkbox or checklist claim, note only
   whether the Diff or Test evidence contradicts it (for example,
   "tests added" with no test in the Diff). Routine-check
   confirmations need no separate output. For asks outside the PR text (a CLA, a
   vouch flow), note only whether the package shows they were not met.
7. **title-describes-change:** the Title, and beside it the one-line
   summary of the Diff.

If a part named in a check's Evidence column is missing from the
package entirely, record "absent" for that check. Do not substitute a
different part. A Repo facts line that states no rule is not absent;
record "no rule".

## Check execution

1. Run the checks in rubric order: plan-fidelity,
   description-matches-diff, test-evidence, repo-checks,
   reviewable-diff, template-and-policy, title-describes-change.
2. Grade each check only against its own pass condition, using only
   the pairing gathered for it. Do not fail a check for a problem
   another check covers. For example, a debug print fails
   reviewable-diff, not plan-fidelity; a description that ignores the
   template fails template-and-policy, not description-matches-diff.
3. Judge what the PR does, not how it is written. Length, headings,
   tone, terseness, and number of commits never decide a grade. A
   two-line description that is accurate and meets the repo's asks
   passes.
4. **Apply the disclosure rule before failing.** For plan-fidelity,
   test-evidence, and repo-checks, an "extra", "missing", "not run",
   or "failed" item with a Description sentence that discloses it and
   gives the reason is not a fail. Only an undisclosed shortfall
   fails.
5. For **plan-fidelity**, fail if any hunk is "extra" with no
   deviation note or disclosure, if any hunk touches something the
   plan lists as not in scope without a deviation note, or if any plan
   step is "missing" with no disclosure.
6. For **description-matches-diff**, fail if any claim is marked
   "contradicted", or if a claim marked "nothing backs this" names
   something a reviewer would expect to see in the package (a test,
   a file, a behavior, a plan step). Before failing, ask: "would this
   claim mislead a reviewer about what the change does, what it
   touches, or how it was tested?" If not, it passes. Loose summaries
   and slightly off counts pass. "Routine" claims pass unless
   contradicted. A confidently worded claim that is backed passes.
7. For **test-evidence**, pass only if at least one planned test is
   "ran, output shown", from after the change, and its output differs
   from the failing Repro output the way the plan expected. Fail if
   every planned test is "claimed, no output" or undisclosed "not
   run", or if the only output shown is from before the change.
8. For **repo-checks**, fail if a check the repo asks for is "not
   mentioned" or "vague" with no disclosed reason, or if the repo asks
   for tests with
   behavior changes and the Diff adds or changes no test and the
   Description does not say why. Pass if read order step 1 recorded
   "no rule" for tests and checks.
9. For **reviewable-diff**, fail on any debug output, commented-out
   code, or stray file. Fail on unrelated edits or formatting churn
   unless a deviation note explains why the fix needs them.
10. For **template-and-policy**, fail if any ask is "missing" with no
    explanation, or if a checklist claim is contradicted by the Diff
    or Test evidence. Pass
    if read order step 1 recorded "no rule" for both the template and
    the policy.
11. Grade `unclear` only for a check whose evidence was recorded as
    "absent". Never use `unclear` because a judgment is hard; decide
    pass or fail and quote the line that decided it.
12. For every grade, record one line: the quote or fact that decided
    it.

## Verdict assembly

1. List the seven grades in rubric order.
2. Apply the verdict rule in `rubric.md`. If every `required` check is
   `pass`, the verdict is accept. If any `required` check is `fail` or
   `unclear`, the verdict is reject.
3. `preferred` checks are reported but never change the verdict.
4. For a reject, the deciding check is the first failing or unclear
   `required` check in rubric order. Quote the line that decided it,
   and say whether it failed or was unclear. Name any other failing
   required checks after it.
5. For an accept, quote the line that decided plan-fidelity, since the
   diff matching the plan is what makes a PR ready to submit.
6. Write the output in the format SKILL.md requires, ending with the
   JSON block.
