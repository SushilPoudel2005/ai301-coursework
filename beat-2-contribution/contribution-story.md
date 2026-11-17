# Contribution: Remove or enforce unused evaluation-contract fields in WebsiteBench

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Sushil Poudel
**GitHub Username:** SushilPoudel2005
**Issue:** https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24
**Pull Request:** pending — will be added once the PR is opened

One repository, built across units 5 to 8, telling the whole story of my real contribution.
I am filling this as I work, not at the end.

---

## Toolkit

Every story section below ends with a Toolkit block covering which tool did the work, what
it produced, what difficulties it presented, and what I changed because of them.

**Unit 5: Built the issue-scout skill, ran two scans, and patched the rubric after the first scan surfaced brand-new solo repos**

I installed the issue-scout skill and wrote a fit profile covering Python, evaluation harnesses, RAG, FastAPI, and backend data work. I built a six-query search strategy and ran the first scan; it returned 536 results and after ranking filters gave me five candidates, every one of them from a repo created in the past week. One was created ten minutes before the scan. The "Repo in use" check was passing on recency alone with no adoption floor, so a solo owner could push today and pass every rubric check. I patched the rubric to require at least 20 stars or at least 2 distinct human authors in the last 5 commits. The second scan rejected three of those repos immediately and left two real candidates: CloudCaptain#199 (docs/curriculum, weak fit) and WebsiteBench#24 (Python evaluation harness, strong fit). I ran the Unit 1 harness regression after the patch and got 19/20, above the bar.

**Unit 6: Voice-guard installation and repo-memory file for WebsiteBench**

I will install the voice-guard skill, review the WebsiteBench contribution guidelines and coding style, and create the repo-memory file that records those norms before I write any code. I will fill this entry with what broke or fell short, and what I changed, when that work is done in Unit 6.

**Unit 7: Ship-gate run on my working branch and gate manifest**

I will run the ship-gate skill against my branch before opening the PR and record the manifest output here. I will note what the gate found, what I had to fix to pass, and anything that surprised me. This entry will be filled in Unit 7.

**Unit 8: PR-monitor and playbook for the open pull request**

I will use the PR-monitor skill to track maintainer feedback on the open PR and draft a response playbook from it. I will record what the monitor read, what it drafted, and what I actually did with the draft when that work is done in Unit 8.

---

## Why I Chose This Issue

I ran my issue-scout skill with a six-query search strategy targeting `rag`, `tool calling`, `evaluation`, `prompt`, `fastapi`, and `laspy` on GitHub. The first scan surfaced 536 results and after de-duplication and ranking filters I had five candidates, but every one of them came from repos created in the past week. That was a red flag: brand-new repos were passing the "Maintainer responsive" check on the fallback route — owner-opened with a good-first-issue label and a recent commit — without any evidence of real maintainer communication with contributors. I added a community-adoption floor to the "Repo in use" rubric — at least 20 stars, or at least 2 distinct human authors in the last 5 commits — and ran a second scan. It immediately rejected three solo repos and left two real candidates.

WebsiteBench#24 stood out as the stronger of the two. It is evaluation-harness work in Python, which is exactly where my learning goals sit. The repo has 511 stars, the issue has zero comments and no assignee, it carries a `good first issue` label, and the fix is bounded: the sealed contract fields `budgets` and `threshold` are defined in every evaluation contract but are never read by the verifier. The maintainer notes they should either be removed or enforced. That is a concrete, well-scoped code-and-docs change with a clear entry point. The only open question before claiming was whether the maintainer had already decided on remove vs. enforce, so I asked in my claim comment rather than assuming. I posted the claim on 2026-11-17: https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24#issuecomment-6963901597.

Looking back at the scout itself, the main shortcoming it exposed is that the `prompt` query is too broad — it matched docs prompts and UI prompts, and a lot of noise came through that channel in both scans. I also noticed the rubric has no explicit threshold for how many sub-items turn a bounded issue into an umbrella; CloudCaptain#199 had three near-identical items and I had to reason through it manually rather than applying a rule. Those are the two things I would update if I ran another scan.

**Toolkit**

- Tool: issue-scout (search-strategy.md with six queries, rubric.md with the patched "Repo in use" criterion), run as two sequential scans after a mid-search rubric patch
- What it produced: scan 1 had four accepts from brand-new solo repos; scan 2 (post-patch) had two real accepts; WebsiteBench#24 was the top fit by both stars and learning-goal alignment
- Difficulties: rubric had no adoption floor, so solo repos with bulk-created issues passed "Repo in use"; the `prompt` query matched too broadly; no umbrella threshold in the rubric made CloudCaptain#199 a judgment call
- What changed: added the 20-star or 2-distinct-author requirement to "Repo in use"; regression ran 19/20 and passed the bar

---

## Understanding the Issue

**Problem Description**

The WebsiteBench evaluation harness uses a contract format to describe what a website evaluation should do. Each contract can include `budgets` and `threshold` fields. The issue reports that these fields are "sealed" — they appear in the contract schema but the verifier code never reads them. Because the verifier ignores them, a user can write any value in those fields and the evaluation produces the same result. The issue also notes that two docstrings in the codebase reference documentation that does not exist.

**Expected Behavior**

Either the verifier should read the `budgets` and `threshold` fields and use their values to gate or shape the evaluation result, or the fields should be removed from the schema so the contract format accurately reflects what the verifier actually does. The two broken docstrings should point to real documentation or be rewritten to be self-contained and accurate.

**Current Behavior**

The fields `budgets` and `threshold` are defined in contracts but silently ignored by the verifier. Any value written in those fields has no effect on evaluation output. Two docstrings reference documentation pages that do not exist.

**Affected Components**

Based on the issue description, the affected files are in the Python evaluation harness: the contract schema definition, the verifier module that reads and processes contracts, and at least two docstrings. I will confirm the exact file paths and line numbers once I clone and read the codebase in Unit 6.

**Toolkit**

- Tool: I read the issue body directly and cross-referenced the scout's scan-output.md grading for WebsiteBench#24
- What it produced: a clear picture of two related problems — dead contract fields and broken docstrings — and the open question of remove vs. enforce
- Difficulties: the issue deliberately leaves the design decision open, so I cannot fully scope the change until the maintainer responds
- What changed: I phrased my claim comment as a question asking for the maintainer's preference before writing any code

---

## Reproduction Process

**Environment Setup**

I will clone TIGER-AI-Lab/WebsiteBench into a fresh directory and set up a Python virtual environment with the dependencies listed in the project's requirements file. I will note the exact Python version, any install friction, and how I verified the project runs correctly before I touch anything. This work will happen in Unit 6, and I will update this section with the actual commands and output when it does.

**Steps to Reproduce**

1. Clone the repo and install dependencies
2. Find a contract file that includes `budgets` and `threshold` fields
3. Run the verifier against that contract with deliberately wrong or extreme values in those fields
4. Observe that the verifier produces the same result regardless of those field values, confirming they are never read

**Reproduction Evidence**

- **Commit showing reproduction:** pending — will link here once I have read the verifier source and confirmed the behavior in Unit 6
- **Screenshots/logs:** not yet available — will add verifier output showing the ignored fields once I run it
- **My findings:** not yet available — will record what I discover when I read the verifier source and test the behavior

**Toolkit**

- Tool: repro-check will be used to document the reproduction in Unit 6
- What it produced: pending
- Difficulties: pending
- What changed: pending

---

## Solution Approach

**Analysis**

Based on the issue description, the root cause is that the contract parsing code defines fields for `budgets` and `threshold` but the verifier never reads them downstream. The fix depends on the maintainer's design preference: enforce means the verifier needs to be updated to read these fields and apply them as constraints on evaluation output; remove means the schema definition and any example contracts that reference them need to be cleaned up. The broken docstrings are a simpler parallel fix. I will trace the exact files and functions once I clone the codebase in Unit 6 and read the verifier source.

**Proposed Solution**

My initial leaning is to remove the unused fields unless the maintainer says otherwise, because removing dead configuration is simpler, easier to review, and leaves no ambiguity about what the verifier does. I will not propose enforcement without the maintainer's direction since it would change evaluation behavior, not just schema shape. I will also fix the two broken docstrings in the same PR. I will not touch any other contract fields or verifier logic beyond what the issue explicitly describes.

**Implementation Plan**

1. Read the verifier source to find which files define and consume contract fields
2. Wait for the maintainer's response to my claim comment before writing any code (remove vs. enforce)
3. Remove or add enforcement for `budgets` and `threshold` per the maintainer's direction
4. Fix the two broken docstrings
5. Update any example contracts in the repo that reference the changed fields
6. Run the test suite and confirm nothing breaks

I will record any deviations from this plan here as they happen in Unit 7.

**Toolkit**

- Tool: plan-check will be used to validate the approach in Unit 7 before I build
- What it produced: pending
- Difficulties: pending
- What changed: pending

---

## Testing Strategy

I will re-run the reproduction steps after my change to confirm the verifier now rejects contracts with the removed fields (or correctly enforces the values, if the maintainer chooses enforcement). I will also run the project's existing test suite before and after my change and record both outputs here. If the project has no test for contract field validation, I will add one. I will not mark testing complete until I have concrete before-and-after output that shows the fix working. This section will be filled with actual test output in Unit 7.

**Toolkit**

- Tool: I will use the repo's test runner (pytest or equivalent) and ship-gate in Unit 7
- What it produced: pending
- Difficulties: pending
- What changed: pending

---

## Implementation Notes

**2026-11-17**

Ran two issue-scout scans. Scan 1 surfaced four accepts from brand-new solo repos. Recognized that the "Repo in use" check needed an adoption floor since recency alone was not enough. Patched the rubric to require 20+ stars or 2+ distinct human authors in the last 5 commits. Ran scan 2 — three repos rejected, two real accepts remaining. WebsiteBench#24 is the stronger pick: 511 stars, Python evaluation harness, zero comments, good-first-issue label, bounded scope. Posted a claim comment asking the maintainer to confirm their preference on remove vs. enforce before I write any code. Waiting for a response.

**[Date to be filled]**

Will be filled in as I do the reproduction and implementation work in Units 6 and 7.

**Toolkit**

- Tool: issue-scout for scouting; direct reading of the WebsiteBench issue page to understand the scope
- What it produced: a clear issue choice and an open maintainer question before any code is written
- Difficulties: none in the planning phase beyond the rubric gap already described in the scan notes
- What changed: rubric patched; claim comment phrased as a question to get maintainer direction first

---

## Pull Request

**PR Link:** pending — will be opened in Unit 7 after the fix is built, tested, and passes ship-gate

**PR Description:** pending — the reproduction and solution sections above will adapt directly into it

**Maintainer Feedback:**

- 2026-11-17: no response yet; I posted the claim comment and am waiting for direction on remove vs. enforce

**Status:** Awaiting maintainer response to claim comment

**Toolkit**

- Tool: pr-precheck and PR-monitor will be used in Unit 8 when the PR is open
- What it produced: pending
- Difficulties: pending
- What changed: pending

---

## Learnings and Reflections

**Technical Skills Gained**

I have not yet built the fix, so the full technical learning will come in Units 6 and 7. From the scouting work in Unit 5 I already learned something concrete: a rubric that checks whether a repo is "in use" needs a community-adoption floor, not just activity recency. A solo owner can push 10 commits today and pass every activity check while the repo has zero other contributors. That is a real insight about how to design scout heuristics, and I will carry it into future searches and into how I think about evaluating open-source health generally.

**Challenges Overcome**

The main challenge in Unit 5 was recognizing that my first scan was surfacing fresh solo repos as good candidates. The rubric was locally consistent — each repo was active and well-scoped — but globally wrong, because community adoption was not being measured at all. The fix was to add a star count or distinct-author check, then run the Unit 1 regression to make sure the patch did not break existing passing cases. It passed 19/20 with no regression on the checks this patch touched.

**What I'd Do Differently Next Time**

I would add a minimum star count or PR merge history filter to the ranking query itself, before grading, so the scout never grades brand-new repos at all. I would also narrow the `prompt` query from the broad single keyword to something like `prompt engineering` or `prompt template` to cut the docs and UI noise that dominated query 4 in both scans.

---

## Resources Used

- WebsiteBench issue #24: https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24 — the issue description, which gave me the exact scope and the open remove-vs-enforce question
- My claim comment posted 2026-11-17: https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24#issuecomment-6963901597 — the starting point of my contribution
- My scout scan output in `~/.claude/skills/issue-scout/scan-output.md` — the ranked candidates and their rubric grades that led me here
- My scout scan notes in `~/.claude/skills/issue-scout/scan-notes.md` — the friction log that identified the rubric gap and drove the patch
