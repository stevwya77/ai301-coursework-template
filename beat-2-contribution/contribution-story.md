# Contribution: Frame on Frame RAM Increase Using PyBindings for 3.1.0 Converting EXR To Uncompressed PNG

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]

**Student:** Stephanie Wyatt

**GitHub Username:** stevwya77

**Issue:** https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613

**Pull Request:** [GitHub PR link, once opened]


One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, a skill that searches GitHub from my fit profile, ranks the results, and grades the top five against my rubric**

I upgraded my Unit 1 issue-select skill into issue-scout by turning my fit profile into gh search queries and ranking rules, then tested it with five scans. The scans exposed real gaps: my rubric passed repos that ban AI-written code, refuse outside PRs, or have almost no users, and my ranking kept surfacing hours-old issues in toy repos instead of my priority projects. I added repo-in-use and accepts-outside-prs checks, tightened unclaimed and newcomer-scope to catch upstream bugs, missing hardware, and maintainers fixing issues themselves, and reordered the ranking so my priority repos come first. By the fifth scan, every candidate came from an active project and every rejection was correct.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my issue-scout skill, which turns my fit profile (C++ and Go first, bug fixes and tests, projects like GrapheneOS, Mayhem, Zulip, Kubernetes, and the EFF) into GitHub searches, ranks the results, and grades the top five against my rubric. Across six scans it graded thirty issues, and almost all were correctly rejected: one repo banned AI-written code, one refused outside PRs, others were upstream bugs, already claimed, or in tiny brand-new repos. I also graded my picks from the course's Issue Selection site, and the scout accepted OpenImageIO #4613 on all six checks. It fits my profile best: it's a C++ bug in an active project with over 2,000 stars, a maintainer (lgritz) labelled it good first issue and invited newcomers to confirm the leak and narrow down its source, I can reproduce it on my Mac with no special hardware, and the project's AI policy only asks for a human author and an Assisted-by line. I hope to learn how to track down a memory leak across the boundary between Python bindings and C++ code. The maintainer warned the full fix "may or may not" be newcomer-sized, so my first goal is a solid reproduction and diagnosis.

Honest read of the scout: the rubric held up. It caught everything I built it to catch, and every rejection I checked was correct. The ranking is the weak part. Ranking my priority repos first keeps filling the top five with unlabelled issues from those repos, so my latest scan graded five issues and none were usable. It also assumed my operating system until I added it to my profile. Next I'd add a drop rule for upstream, invalid, wontfix, duplicate, and ai-started labels, and require a good first issue or help wanted label on the priority-repo queries.

Claim comment: https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6788821588

**Toolkit**

- Tool: issue-scout, my search strategy and rubric. I ran six scans plus a grade run on my Issue Selection picks.
- What it produced: an accept verdict on OpenImageIO #4613 with all six checks passing (2,391 stars, maintainer reply within ~1.5 hours on a recent issue, no claims or PRs, a triaged single bug, outside PRs welcome, AI allowed with disclosure).
- Difficulties: the ranking keeps sending unlabelled priority-repo issues to grading, so my last scan found nothing usable; GitHub rate-limited several queries; the scout guessed my OS until I listed it.
- What changed: I added repo-in-use and accepts-outside-prs checks, tightened unclaimed and newcomer-scope, and listed my machines in my profile. Next I'd add label-based drop rules and require beginner labels on priority-repo queries.

---

## Understanding the Issue

**Problem Description**

When a Python script uses OpenImageIO's Python bindings to read a large EXR and write it as an uncompressed PNG, memory usage climbs with every image and is never released. The reporter (OpenImageIO 3.1.0, Python 3.10.6, Windows) saw about 1 GB added per frame on roughly 20,000 x 2,000 images, so a batch job over many frames eventually runs out of RAM. The same conversion with oiiotool doesn't leak, and writing TIFF instead of PNG from Python doesn't leak, which points at the PNG writer as called from Python.

**Expected Behavior**

Memory should stay roughly flat across frames. Once a frame's ImageOutput is closed and the pixel data goes out of scope, its memory should be freed before the next frame starts.

**Current Behavior**

Each converted frame adds about 1 GB of memory that is never freed, so usage grows steadily until the process is stopped. The maintainer asked whether this still happens in current versions, which hasn't been confirmed yet.

**Affected Components**

Suspected, to confirm during reproduction: the Python bindings for ImageInput and ImageOutput (read_image and write_image), and the PNG output plugin in src/png.imageio/. The leak may be in how the bindings pass pixel buffers to the PNG writer, or in the PNG writer itself when compression is set to 0.

**Toolkit**

- Tool: issue-scout's grade read-out, plus Claude to help me read the issue thread and summarize it.
- What it produced: a summary of the bug, the maintainer's triage note, and a list of likely components to investigate.
- Difficulties: the components are guesses until I reproduce the leak and trace it; nobody has confirmed it still happens on current versions.
- What changed: nothing yet. Reproduction will confirm or correct this list.

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
