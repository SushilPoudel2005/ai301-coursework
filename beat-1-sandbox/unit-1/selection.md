# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/12

**Verdict output**

My skill's live-mode output for issues #12, #11 and #15 (graded together). #12 is the top-ranked accept and the one I chose.

`````
## Ranked read-out

All three issues are in the scoped repo, `codepath/pathreview-ai301-fa26-s1`. All three are accepted, but #15 is a weak accept that depends on a house-rule reading (see the end).

1. **#12 (snapshot tests for prompt templates)** is the best fit. It is a contained test-writing task in the RAG code (`tests/unit/test_prompt_templates.py`), estimated at 3–5 hours. The issue carries a `devops` label, but the work is only a test. A classmate (Sroberts03) claimed it on Sep 28, 47 days ago, with no PR linked. Under the house rule that doesn't block it.
2. **#11 (explain the hybrid retrieval scoring formula in `docs/ARCHITECTURE.md`)** is cleanly open: no comments, no claims, no PRs. It is a 2–3 hour docs task. It fits your want for a small, well-scoped issue and for reading RAG code, but it gives less test-writing practice.
3. **#15 (session state not cleared between reviews)** is a contained bug fix in the agent code. It ranks last because classmate shanekellyreyes already has an open PR, #202, that says "Closes #15". It passes only through the house rule.

Per-check results:
- **Issue open:** all three are open.
- **Maintainer commits:** the newest commit is Sep 16, which is 59 days before today (Nov 14). It was authored by Aburke225, a human. This is under the 90-day limit.
- **Maintainer responsive:** I couldn't check reply times. The GitHub API rate limit ran out and the issue list doesn't show reply badges. All three pass on the rubric's third route: the issue was opened by a COLLABORATOR (Aburke225) and carries `good first issue`, and the newest commit is within 60 days. That margin is only about one day. If the next commit slips, this fails for all three.
- **Repo in use:** the repo page shows no archived banner, and the Sep 16 commit is within 90 days. I couldn't read the archived flag from the API, only the banner. There are no releases, which doesn't matter because the push is recent.
- **Bounded scope:** all three pass. Each is a single-PR task with no tracking list, no unresolved design debate and no bot opener.
- **AI contribution policy:** `docs/CONTRIBUTING.md` says nothing about AI, and the repo has no `AI_POLICY.md`, so silence passes. I didn't find a root `CONTRIBUTING.md` or an `AGENTS.md`. I didn't read the PR template.
- **Preferred checks:** all three have `good first issue` and `tier-1` labels from a maintainer. #12 and #15 name files and describe the fix. #11 states its goal and effort but has no checklist.

On #15, the rubric says an open linked PR fails "Unclaimed". I passed it because the house rule says to ignore claim signals from other students, and I read an open PR as one of those. If you read the house rule as covering only claim comments, #15 is a reject. The page also disagrees with itself: the header shows PR #202 but the sidebar says "No branches or pull requests". Taking this issue would mean competing with a ready PR, so I'd choose #12 or #11.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/12",
    "checks": [
      {"name": "Issue open", "grade": "pass", "evidence": "state: open"},
      {"name": "Maintainer commits", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 (59 days old), authored by human Aburke225"},
      {"name": "Maintainer responsive", "grade": "pass", "evidence": "Reply sample unavailable; opener Aburke225 is COLLABORATOR, label good first issue, newest commit 59 days old (<=60)"},
      {"name": "Repo in use", "grade": "pass", "evidence": "No archived banner; last commit/push 2026-09-16 (<=90 days)"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Single task: snapshot tests in tests/unit/test_prompt_templates.py; no debate or maintainer-flagged core-internals work"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "No assignee, no linked PR; only claim comment 2026-09-28 (47 days ago) and house rule ignores classmate claims"},
      {"name": "AI contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI restriction; no AI_POLICY.md"},
      {"name": "Good-first-issue signal", "grade": "pass", "evidence": "Labels: good first issue, tier-1; opened by COLLABORATOR"},
      {"name": "Clear acceptance", "grade": "pass", "evidence": "Tests must fail when a template changes without a version bump; file and effort named"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/11",
    "checks": [
      {"name": "Issue open", "grade": "pass", "evidence": "state: open"},
      {"name": "Maintainer commits", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 (59 days old), authored by human Aburke225"},
      {"name": "Maintainer responsive", "grade": "pass", "evidence": "Reply sample unavailable; opener Aburke225 is COLLABORATOR, label good first issue, newest commit 59 days old (<=60)"},
      {"name": "Repo in use", "grade": "pass", "evidence": "No archived banner; last commit/push 2026-09-16 (<=90 days)"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Add one section to docs/ARCHITECTURE.md explaining the scoring formula with an example; estimated 2-3 hours"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "No assignee, no comments, no linked PR"},
      {"name": "AI contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI restriction; no AI_POLICY.md"},
      {"name": "Good-first-issue signal", "grade": "pass", "evidence": "Labels: good first issue, docs, rag, tier-1"},
      {"name": "Clear acceptance", "grade": "pass", "evidence": "Body asks for a section explaining the formula and default weights with an example"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/15",
    "checks": [
      {"name": "Issue open", "grade": "pass", "evidence": "state: open"},
      {"name": "Maintainer commits", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 (59 days old), authored by human Aburke225"},
      {"name": "Maintainer responsive", "grade": "pass", "evidence": "Reply sample unavailable; opener Aburke225 is COLLABORATOR, label good first issue, newest commit 59 days old (<=60)"},
      {"name": "Repo in use", "grade": "pass", "evidence": "No archived banner; last commit/push 2026-09-16 (<=90 days)"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Contained fix: clear ContextManager cache and SessionStore state at start of run(); 3-4 hours, no maintainer debate"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "No assignee; PR #202 (shanekellyreyes, open, 'Closes #15') and the claim comments are classmate claim signals the Path Review house rule says to ignore. Reject if the house rule is read as covering comments only."},
      {"name": "AI contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI restriction; no AI_POLICY.md"},
      {"name": "Good-first-issue signal", "grade": "pass", "evidence": "Labels: good first issue, agent, tier-1"},
      {"name": "Clear acceptance", "grade": "pass", "evidence": "Body states the symptom (second run returns first run's cached result) and the suggested fix"}
    ],
    "verdict": "accept"
  }
]
```
`````

---

## Eval iterations

**Run history**

1. Full run, first rubric: 17/20. Disagreements: issue-14 (gold accept, I rejected), issue-19 (gold accept, I rejected), issue-20 (gold reject, I accepted). Category line: `clear-accept 6/8`, `scope 3/4`.
2. Partial re-run with `--only issue-14,issue-19,issue-20,issue-01,issue-07`: 5/5. Partial, so not scored.
3. Full run: 20/20. Saved to `eval-run.txt`, later overwritten.
4. Live mode on Path Review issues #11, #12 and #15 rejected all three, so I loosened one check and tightened another. Full run: 19/20, with issue-15 (gold reject) now accepted.
5. Full run after making the abandoned-attempts scope rule explicit: 20/20. This is the run in `eval-run.txt`.

Last score: 20/20.

**Issue analysis**

issue-20 (excalidraw, "Add company logo shape to the toolbar", #11811). My first rubric decided **accept**: every required check passed, and the failed-check column was empty. The gold label is **reject**. My rubric looked at the repo (commits, release, push, policy) and at claim signals, but nothing in it looked at who opened the issue. The issue was opened by `cursor[bot]` (author_association NONE), has no labels, and no maintainer commented. Its body says "Logo asset TBD." That makes it an auto-generated request that nobody on the team has vetted, and its key input does not exist yet. I added this to Bounded scope: fail if the issue "was opened by a bot account (username ending `[bot]`) with no maintainer comment or maintainer-applied label, or its key input is still "TBD"". After that change issue-20 rejects and the rest of the set was unchanged.

**Check rationale**

Check quoted from `tools/issue-select/rubric.md`, exactly as it is now:

| Maintainer responsive | "maintainer first-response sample" in repo facts, plus author_association (OWNER/MEMBER/COLLABORATOR) on comments in this thread | Ignore sampled issues opened fewer than 7 days before the capture date (too young for "no reply" to mean anything). Of the rest, at least 1 got a maintainer reply within 45 days, OR a maintainer commented in this thread, OR the issue itself was opened by an OWNER/MEMBER/COLLABORATOR and carries a `good first issue` label AND the newest default-branch commit is at most 60 days old (a maintainer who wrote and labeled the issue recently is engaged even if no one has replied yet). If no sampled issue is old enough to count, pass only when the commit check shows 3+ commits by humans in the last 5. A sample where every countable issue is "no maintainer comment" fails. | required |

The first two sentences exist because of issue-14: its only sampled issue was opened one day before the capture date, so "no maintainer comment" said nothing about the maintainers. The third route (maintainer-opened, `good first issue`, recent commit) exists because live mode on the Path Review repo rejected all three candidates. The only maintainer is course staff, and nobody replies in these threads. Its requirement of a commit within 60 days keeps it from passing a dead repo.

**Trade-offs**

The third route gives up a real check. A repo where the maintainer opens and labels issues but never answers anyone now passes this check, and the reply-latency sample is skipped. The live run showed this directly: all three Path Review candidates passed "Maintainer responsive" with the grader noting "Reply sample unavailable". I accept it because that repo is a classroom, where review is done by staff against submitted PRs. In a real open-source repo I would want the reply sample to carry more weight. The evidence that nothing else broke is the final full run: 20/20 with every category matched, including `dead-repo 3/3`. The commit-recency limit was also loosened from 60 to 90 days in the same edit, and on live data the newest commit (2026-09-16) is 59 days old, so the 60-day limit on the third route will stop passing within a day or two unless the repo gets a new commit.

---

## Selection rationale

**Selection rationale**

1. Fit: #12 asks for snapshot tests for the prompt templates, a contained Python test-writing task in `tests/unit/test_prompt_templates.py`. It matches my goal of getting better at writing tests and reading the RAG code, and it is small enough for part-time work over about a week. #11 is docs only, which teaches me less. #15 already has an open PR.
2. What the verdict got right: the issue is open and unassigned, bounded to one test file, labelled `good first issue` and `tier-1`, with clear acceptance criteria and no AI-contribution restriction. What it could not weigh: a classmate (Sroberts03) commented a claim on 2026-09-28, and a comment on the issue says the existing `test_template_snapshot_content_hash` never compares against a baseline, which changes what the tests need to do. The rubric also could not tell me whether the maintainer will actually review quickly. The live run passed that check on a fallback route.
3. Difficulty claiming: another student has already claimed it, so I may be working alongside them. The Path Review house rule says shared issues are fine and credit attaches to the pull request, so I plan to claim anyway. I will not comment until Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
