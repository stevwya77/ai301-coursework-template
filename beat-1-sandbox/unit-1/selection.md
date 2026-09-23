# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

```
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69
```

**Verdict output**


```json
1. #69 — Output parser crashes on a top-level JSON array fallback
Best fit: rag/generator/output_parser.py is parser-and-format work, which your profile names directly. Bounded to two files with a seeded xfail marker (H-02) that tells you exactly when you're done. Two classmates have claimed it; under the house rule that costs you nothing.
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Most recent non-bot commit 2026-09-16 by Aburke225 (6 days); COLLABORATOR Aburke225 replied on issues #52 and #43 on 2026-09-16, within 60 days."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; repo has zero PRs so no linked PR; the only claims are classmates Yina-Mu and tonybuii2003 (author_association NONE), which the Path Review house rule says do not block."},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR Aburke225, labeled 'good first issue'/'bug'/'tier-1', names two files and an xfail marker (H-02) to remove, est. 2-4 hours; not an umbrella, question, or unendorsed feature request."},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI/LLM/generated-code clause and no AI_POLICY.md exists; a repo with no stated policy passes."}
    ],
    "verdict": "accept"
  }
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```json
All three graded independently against rubric.md. Evidence is live from the repo (today 2026-09-22).

Shared repo facts
- Last default-branch commit: 2026-09-16 by Aburke225 (human COLLABORATOR) — 6 days old.
- Maintainer response sample: Aburke225 (COLLABORATOR) replied on #52 and #43 on 2026-09-16, within 60 days.
- Contribution policy: docs/CONTRIBUTING.md (189 lines) has no AI clause; no AI_POLICY.md, no AGENTS.md; PR template asks for no AI disclosure → silence passes.
- The repo has zero pull requests, open or closed, so no issue carries a linked PR.
- Path Review house rule applied: classmates' claim comments (all NONE association) do not block.

Accepted, in fit order

1. #69 — Output parser crashes on a top-level JSON array fallback
Best fit: rag/generator/output_parser.py is parser-and-format work, which your profile names directly. Bounded to two files with a seeded xfail marker (H-02) that tells you exactly when you're done. Two classmates have claimed it; under the house rule that costs you nothing.

2. #68 — Keyword search raises ZeroDivisionError when the index is empty
Second: KeywordSearcher.index() / BM25 is index-and-data-structure territory, adjacent to your target areas, and the fix mirrors an empty-case guard search() already has. Same two-file, xfail-marked shape (H-01).

3. #72 — verify_password raises UnknownHashError on malformed hashes
Third: the smallest change of the three (1–2 hours, fail-closed on one exception), but auth error handling doesn't touch any area on your list, so it ranks below the other two despite the size advantage.

No rejections. All four required checks passed on every candidate; all three are Python, so none gives you the Go/C/Rust reps you're after.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Most recent non-bot commit 2026-09-16 by Aburke225 (6 days); COLLABORATOR Aburke225 replied on issues #52 and #43 on 2026-09-16, within 60 days."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; repo has zero PRs so no linked PR; the only claims are classmates Yina-Mu and tonybuii2003 (author_association NONE), which the Path Review house rule says do not block."},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR Aburke225, labeled 'good first issue'/'bug'/'tier-1', names two files and an xfail marker (H-02) to remove, est. 2-4 hours; not an umbrella, question, or unendorsed feature request."},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI/LLM/generated-code clause and no AI_POLICY.md exists; a repo with no stated policy passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Most recent non-bot commit 2026-09-16 by Aburke225 (6 days); COLLABORATOR Aburke225 replied on issues #52 and #43 on 2026-09-16, within 60 days."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; repo has zero PRs so no linked PR; sole claim is classmate acordero4852 (author_association NONE) on 2026-09-19, which the Path Review house rule says does not block."},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR Aburke225, labeled 'good first issue'/'bug'/'tier-1', names rag/retriever/keyword_search.py plus its test and an xfail marker (H-01), est. 2-4 hours; no design debate in thread."},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI/LLM/generated-code clause and no AI_POLICY.md exists; a repo with no stated policy passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Most recent non-bot commit 2026-09-16 by Aburke225 (6 days); COLLABORATOR Aburke225 replied on issues #52 and #43 on 2026-09-16, within 60 days."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; repo has zero PRs so no linked PR; sole claim is classmate sseid4 (author_association NONE) on 2026-09-22, which the Path Review house rule says does not block."},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR Aburke225, labeled 'good first issue'/'bug'/'tier-1', names core/security.py plus its test and an xfail marker (H-05), est. 1-2 hours; a single fail-closed change, not core internals."},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI/LLM/generated-code clause and no AI_POLICY.md exists; a repo with no stated policy passes."}
    ],
    "verdict": "accept"
  }
]

```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

```
1. agreement: 15/20 scored items  (bar: 18/20: below the bar)
2. agreement: 19/20 scored items  (bar: 18/20: PASS)
```

**Issue analysis**

```
item      gold    verdict  agree  note
issue-15  reject  accept   NO     graded accept
```

**Check rationale**

```
| unclaimed | Repo facts: `this issue: assignees:` and `linked PRs:` with state, plus the Comments section | passes only if all three hold: `assignees:` is none; no linked PR is in the open state (a closed or merged PR is an abandoned or finished attempt, not a live claim); and the thread holds no live claim. A claim comment ("I'll take this", "can I work on this", "working on this", or a `/assign` command) counts as live unless a human maintainer (owner, member, or collaborator) commented later in the thread with no sign the claimer is still on it. A bot comment never clears a claim | required |

I wanted to differentiate between humans and bots, and I also wanted to ensure that dead ends were still up for grabs if not at the fault of the maintainer
```

**Trade-offs**

```
A tougher issue that leads to two previous contributors abandoning looks just as free as one nobody has touched. Nothing else in the rubric reads linked pr state, so it wont ever get flagged
```

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

```
1. I put in the scope and actively sought out systems/backend adjacent issues to fix.
2. The verdict was correct on most things, although it became unclear if there were multiple list items like on issue-01
3. No perceived undue hardship, although it does look popular. I know that doesn't affect anything but it was swaying me to maybe pick the second/third option if the rules were different.
```
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
