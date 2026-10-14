# Evidence guide: where evidence lives in a PR package

Each eval package has these sections: `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Plan context` (the accepted plan, with the
repro evidence it built on), and `## Candidate PR`, which holds
`### Title`, `### Description`, `### Commits`, `### Diff`, and
`### Test evidence`.

In live mode (grading your own PR), the same evidence lives in:

| Eval section | Live mode |
|---|---|
| Repo facts | `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the scoped repo |
| Issue, Thread highlights | the GitHub issue page and its comments |
| Plan context | `plan.md`, including `## Deviations`, plus your posted repro comment (or the house repro pack) |
| Title, Description | `pr_draft.md`: line 1 is the title, the rest is the description |
| Commits | `git log main..HEAD` |
| Diff | `git diff main...HEAD` |
| Test evidence | `test_evidence.md` |

Worked example throughout: `calib-01` (delta #2211, removing a
duplicate theme key).

## Plan fidelity (harness category: silent-drift)

Rubric checks: plan-fidelity, description-matches-diff.

**Where it lives**
- **What the plan promised:**
  - Eval: `## Plan context`. Look for the "Plan:" steps, the "Files:"
    line, the "Not in scope:" line, and any deviation note.
  - Live: the Scope, Files, and Approach sections of `plan.md`, and
    `## Deviations`.
- **What actually changed:**
  - Eval: `### Diff`. Each `--- a/` / `+++ b/` pair is one changed
    file; each `@@` block is one hunk.
  - Live: `git diff main...HEAD`. Use `git diff main...HEAD --stat` for
    the list of changed files.
- **What the PR says it does:**
  - Eval: `### Title`, `### Description`, and the messages in
    `### Commits`.
  - Live: `pr_draft.md` and `git log main..HEAD`.

**What good looks like**
- Every changed file is on the plan's Files line or in a deviation
  note, and every plan step shows up as a hunk, or the Description
  says why it doesn't. In calib-01 the plan says "Files:
  `themes.gitconfig`" and step one is "remove the first, shadowed line";
  the Diff changes only `themes.gitconfig` and removes exactly that
  line.
- Every claim in the Description is visible in the Diff or the Test
  evidence. calib-01 claims "rendering is unchanged", and the Test
  evidence shows `--get-all` still prints `#84786A ul`.
- **Silent drift, more than the plan:** an extra file, extra behavior,
  or anything the plan lists as not in scope, with no deviation note.
  In calib-01, touching a second theme would be drift, because the plan
  says "Not in scope: any other theme."
- **Silent drift, less than the plan:** a plan step with no hunk and no
  mention in the Description.
- **Silent drift in the description:** the Description claims something
  the Diff contradicts or doesn't show, such as "only touches X", "adds a
  test", or "fixes #N" when the Diff doesn't.
- An honest deviation, recorded in the plan or disclosed in the
  Description with a reason, re-ties a mismatch. It is not drift.

## Test evidence (harness category: not-tested)

Rubric checks: test-evidence, repo-checks.

**Where it lives**
- **The proof:**
  - Eval: `### Test evidence`.
  - Live: `test_evidence.md`.
- **What it must be compared to:**
  - The plan's "Test plan:" line in `## Plan context` (live: the Test
    plan section of `plan.md`), and the failing output in the Repro
    evidence.
- **The repo's own checks:**
  - Eval: the pull-requests and contribution policy lines in
    `## Repo facts` (for example "asks for tests with behavior
    changes").
  - Live: the checks the PR template lists (`make test-unit`,
    `make test-integration`, `make lint`, `make typecheck`) and their
    output in `test_evidence.md`.

**What good looks like**
- Real output, from after the change, for each test the plan named,
  and that output differs from the failing Repro output the way the
  plan expected. In calib-01 the repro failed with
  `DuplicateOptionError`; the Test evidence re-runs the same
  `configparser` command and shows `parsed OK`, which is exactly the
  "clean parse" the test plan expected.
- The repo's own checks were run and their output is shown. A failing
  check, or one that can't run, counts when it is reported honestly
  with the reason. In live mode, `make test-integration` printing "no
  tests ran" is expected and should be pasted as-is.
- When the repo asks for tests with behavior changes, the Diff adds or
  updates a test, or the Description says why it doesn't.
- **Not tested looks like:** "tests pass" or "verified it works" with no
  output; output that doesn't exercise the changed code; only
  before-the-change output; or a planned test silently skipped.

## Diff quality (harness category: unreviewable)

Rubric checks: reviewable-diff, title-describes-change.

**Where it lives**
- Eval: every hunk in `### Diff`, and the commit messages in
  `### Commits`.
- Live: `git diff main...HEAD` hunk by hunk, plus
  `git diff main...HEAD --stat` to spot files that shouldn't be there,
  and `git log main..HEAD`.

**What good looks like**
- The fix is visible at a glance, and every hunk serves the planned
  change. calib-01 is one hunk that deletes one line.
- The Title tells a reviewer what the Diff changes. calib-01's title
  names the setting, the theme, and the action.
- **Debris to look for:**
  - debug leftovers (`print(`, `console.log(`, `dbg!`, `breakpoint()`,
    `// TODO remove`);
  - commented-out code blocks;
  - stray or generated files (lockfiles, build output, `.DS_Store`,
    editor configs, scratch scripts);
  - whitespace, import reordering, or reformatting of lines the fix
    doesn't need;
  - drive-by edits to unrelated functions or files.
- An unrelated hunk is acceptable only when a deviation note explains
  why the fix needs it.

## Standards and comms (harness category: standards-wall)

Rubric check: template-and-policy.

**Where it lives**
- **The repo's asks:**
  - Eval: the "pull requests:" and "contribution policy" lines in
    `## Repo facts`.
  - Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`
    (and any AI policy file).
- **Maintainer direction:** OWNER, MEMBER, and COLLABORATOR comments in
  `## Thread highlights` (live: the issue's comments).
- **Where the PR meets them:** `### Title` and `### Description` (live:
  `pr_draft.md`).

**What good looks like**
- Every explicit ask is met with real content, or the Description says
  why it isn't: required template sections filled in, checkboxes ticked
  only when true, the issue referenced in the requested form ("Closes
  #N", "fixes #N"), the title in the requested format, and an AI-use
  disclosure wherever the policy asks for one. In live mode, Path
  Review's course rules also ask for the AI disclosure under Notes for
  Reviewers.
- Asks that live outside the PR text (signing a CLA, a vouch flow) count
  only if the package shows they were not met.
- calib-01 has no PR template and no AI policy, and its contribution
  docs ask that PRs stay small and reference the issue. The Description
  references #2211 and the Diff is one line, so it is compliant.
- **Standards wall looks like:** a required template section missing or
  left as placeholder text; a box ticked that the Test evidence doesn't
  back; no issue reference where one is required; no AI disclosure
  where the policy requires one; or a title that ignores a stated
  format. Boilerplate that would read the same on any PR does not count
  as filling a section.
- Whether the Description's claims match the Diff is plan fidelity,
  above, not this family.
