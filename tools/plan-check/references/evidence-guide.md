# Evidence guide: where evidence lives in a plan package

Each eval package has six sections: `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Repro evidence`, `## Candidate plan`, and
`## Candidate plan comment`. In live mode (grading your own plan), the
same evidence lives in `plan.md`, `comment.md`, the GitHub issue page,
your posted repro comment (or the staff repro pack), and the repo's
CONTRIBUTING.md.

## Diagnosis and grounding

**Where it lives**
- **The cause:**
  - Eval: the first lines of `## Candidate plan`, usually a "Cause:"
    sentence.
  - Live: the Diagnosis section of `plan.md`.
- **What the cause must explain:**
  - Eval: `## Repro evidence`. Look at its Steps, the failing output,
    the Expected/Actual lines, and especially any **Control runs** and
    debug output.
  - Live: your posted repro comment.
- **Maintainer culprits:** OWNER, MEMBER and COLLABORATOR lines in
  `## Thread highlights`.

**What good looks like**
- The stated cause explains the failing behavior and also explains why
  each control run behaved differently.
- If a control run removes the blamed component and the bug persists,
  or keeps the component and the bug disappears, the cause is
  contradicted.
- If a maintainer named a culprit, a good diagnosis matches it or
  explains why it differs.

## Scope

**Where it lives**
- The plan's "Change:" text and its "In:" and "Out:" lines, plus every
  file or component the plan names.
  - Eval: these are in `## Candidate plan`.
  - Live: the Scope and Files sections of `plan.md`.
- The yardstick is the reported behavior in `## Issue`.

**What good looks like**
- One change aimed at the reported behavior, with an explicit
  out-of-scope line.
- Every named file has a reason tied to the issue.
- Red flags: "while I'm here" cleanups, renames, framework or dependency
  upgrades, data migrations, or a fix wrapped inside a redesign.

## Executability

**Where it lives**
- The file paths, function or callback names, and the verb describing
  the edit.
  - Eval: in `## Candidate plan`.
  - Live: the Files and Approach sections of `plan.md`.

**What good looks like**
- The plan names where the change goes (a file, plus a function or
  callback when the file is large) and what the change is.
- Example: "in `pkg/gui/controllers/sync_controller.go`, add the commits
  context to the push callback's refresh scope."
- "Improve the handling" or "fix the logic" with no location fails.

## Test plan

**Where it lives**
- **The test:**
  - Eval: the "Test:" lines of `## Candidate plan`.
  - Live: the Test plan section of `plan.md`.
- **The baseline it must be compared to:** the Steps and the failing
  output in `## Repro evidence`.

**What good looks like**
- The plan re-runs the repro steps (or the same inputs through the real
  code).
- It names an observable result that would differ from the failing
  output, such as an exit code, a printed value, or a UI state at a
  named step.
- Bonus: it re-runs the control case, or covers a sibling path that
  shares the same code.

## Honesty

**Where it lives**
- **Certainty claims:** sentences like "the cause is", "confirmed",
  "this fixes", "will work". These appear in `## Candidate plan` and
  `## Candidate plan comment`.
- **Stated unknowns:** risk, unknown, or "not yet checked" lines.
- Live: the Risks and unknowns section and `## Deviations` in
  `plan.md`, plus `comment.md`.

**What good looks like**
- Every "confirmed" or "reproduced" claim is backed by something in the
  Repro evidence.
- Anything the evidence doesn't show is named as an unknown, along with
  how it will be checked.
- After the build, `## Deviations` records what changed from the plan
  and why, or says plainly that nothing changed.

## Comms

**Where it lives**
- **The words being graded:**
  - Eval: `## Candidate plan comment`.
  - Live: `comment.md`.
- **Maintainer direction:** OWNER, MEMBER and COLLABORATOR lines in
  `## Thread highlights` (live: the issue's comments). Look for a named
  culprit, a patch or test binary, a requested approach, or a "please
  don't".
- **Repo conventions:** the `contribution policy` line in
  `## Repo facts` (live: CONTRIBUTING.md and any AI_POLICY.md). Look for
  AI-use disclosure, a "comments must be human-written" rule, PR or
  review limits, and template asks.

**What good looks like**
- The comment visibly engages with what the thread already
  established. It builds on a maintainer's culprit or patch, or says
  why it takes a different route; it doesn't propose something the
  maintainer already ruled out.
- It meets any disclosure rule the policy states.
- Boilerplate that would read the same on any issue ("I'd like to work
  on this, here's my plan") fails when the thread has given direction.
