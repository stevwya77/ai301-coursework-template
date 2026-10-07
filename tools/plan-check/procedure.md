# Procedure: how this skill grades a plan package

## Read order

Read every part before grading any check. The plan is read after the
evidence on purpose: the diagnosis and test-plan checks compare the plan
against the evidence, and reading the plan first anchors you to its
story.

1. **Repo facts.** Note the contribution policy line word for word,
   especially any AI-use or disclosure rule and any comment-format rule.
   If it says "no stated AI policy", note that no disclosure rule
   applies.
2. **Issue.** Note the reported behavior in one sentence (what happens,
   and what should happen instead). This sentence is the yardstick for
   the scope check.
3. **Thread highlights.** Note every comment from an OWNER, MEMBER or
   COLLABORATOR that gives direction: a suspected culprit, a requested
   approach, a request to test something, a "don't do X", or an open PR
   or patch. Write down the date, the author and what they asked for.
   Comments marked NONE or CONTRIBUTOR are context, not direction.
4. **Repro evidence.** Note the steps, the failing output, and every
   control run. For each control run, note the one thing it changed and
   what that rules in or rules out.
5. **Candidate plan.** Read it once in full.
6. **Candidate plan comment.** Read it once in full.

In live mode the same six parts are: the repo's CONTRIBUTING.md and any
AI policy file (1), the issue body (2), the issue's comments (3), the
student's posted repro comment, or the staff repro pack for a house
issue (4), `plan.md` (5), and `comment.md` (6).

## Evidence gathering

For each check, pull these facts and record them as short quotes:

1. **diagnosis:** the plan's cause sentence, and beside it the Repro
   evidence lines it must explain. Include every control run and any
   debug output, plus any maintainer culprit noted in step 3 of the read
   order.
2. **scope:** the plan's change, its in/out statements, and every file
   or component it names. List each named change separately so each can
   be tested against the issue sentence.
3. **executable:** the place of the change (file, function, or
   callback) and the action (what will be added, removed or changed
   there).
4. **test-plan:** the plan's test section, and the expected result it
   names. Next to it, put the failing output from the Repro evidence.
5. **honesty:** every sentence in the plan and the comment that claims
   certainty ("the cause is", "confirmed", "this fixes", "will work"),
   and every sentence that names a risk or unknown.
6. **thread-convention:** the full comment text, the maintainer
   direction noted in read order step 3, and the policy rule noted in
   read order step 1.

If a part named in a check's Evidence column is missing from the
package entirely, record "absent" for that check. Do not substitute a
different part.

## Check execution

1. Run the checks in rubric order: diagnosis, scope, executable,
   test-plan, honesty, thread-convention.
2. Grade each check only against its own pass condition, using only the
   facts gathered for it. Do not fail a check because of a problem that
   another check already covers. For example, a wrong cause fails
   diagnosis, not test-plan.
3. Judge what the plan would do, not how it is written. Length, headings,
   tone and terseness never decide a grade. A three-line plan that names
   the file, the change and the expected result passes executable.
4. For **diagnosis**, test the cause against each control run. If a
   control run shows the failure goes away while the blamed component is
   unchanged, the cause is contradicted: fail. If a maintainer has pointed
   at a culprit and the plan blames something else without addressing
   that, fail.
5. For **scope**, ask of each change listed in gathering step 2: "would
   the issue sentence still be fixed without this?" If yes for any
   change that is more than a few lines of supporting edit (a refactor,
   rename, migration, upgrade, or redesign), fail.
6. For **executable**, ask: "could someone new to the issue open the
   named file and make the first edit without asking the author
   anything?" If the place or the action is missing, fail.
7. For **test-plan**, pass only if the expected result is something you
   could see in output and it differs from the failing output in the
   Repro evidence. "Confirm it works" or "run the test suite" with no
   expected result fails.
8. For honesty, check each certainty claim against the Repro evidence and Thread highlights. A claim the evidence supports passes, however confidently it is worded. Fail only a claim that states more than the evidence shows and isn't named as an unknown.
9. For **thread-convention**: if maintainer direction exists, the comment
   must follow it or say why it doesn't; ignoring it fails. If the policy
   states a disclosure or human-written rule, the comment must meet it.
   If neither exists, pass.
10. Grade `unclear` only for a check whose evidence was recorded as
    "absent". Never use `unclear` because a judgment is hard; decide
    pass or fail and quote the line that decided it.
11. For every grade, record one line: the quote or fact that decided it.

## Verdict assembly

1. List the six grades.
2. Apply the verdict rule in `rubric.md`. If every `required` check is
   `pass`, the verdict is accept. If any `required` check is `fail` or
   `unclear`, the verdict is reject.
3. `preferred` checks are reported but never change the verdict.
4. For a reject, name the deciding check or checks. Quote the line that
   decided each one, and say whether it failed or was unclear.
5. For an accept, quote the line that decided the diagnosis check, since
   it carries the most weight.
6. Write the output in the format SKILL.md requires, ending with the
   JSON block.
