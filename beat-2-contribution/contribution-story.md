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

**Unit 6: Installed voice-guard, patched scope check to scope-feasible, re-graded picks, wrote CLAUDE.local.md, and reproduced the issue by reading the source**

I installed the voice-guard hook (a PreToolUse Bash hook that runs `check_voice.py` before every `gh issue/pr comment/create` command), promoted the voice guide to its canonical home at `~/.claude/voice-guard/voice-guide.md`, and added an Upstream section with rules for claims, questions, and pings. I replaced the "Bounded scope" check in my rubric with the "scope-feasible" row from the Unit 6 patch, which moves open design edges from reject to a difficulty note (straightforward / moderate / ambitious). Re-grading WebsiteBench#24 with the patched rubric gives `accept`, difficulty `moderate` (the files are named, but the remove-vs-enforce decision requires maintainer direction first). I forked and cloned the repo, confirmed there is no CONTRIBUTING.md, and wrote `CLAUDE.local.md` with the conventions I could infer from CI and code style, the exact subsystem (case_protocol.py, visual-suite.json, evaluation-contract.json), and the ruff lint commands. The reproduction itself needed no install — I read the source and ran grep to confirm all three findings: `threshold` in visual-suite.json checkpoints is never read (the scorer uses `VISUAL_SSIM_PASS = 0.99` directly), four of the five `budgets` subfields are unused, and the `case_protocol.py` module docstring cites two documentation files that do not exist. Repro-check graded the claim and repro together as accept. The only friction: repro-check flagged that my claim comment was posted before the exploration and therefore promises future work I had already done — the tense was misleading in hindsight.

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

Three distinct problems, all confirmed by reading the source after cloning in Unit 6:

1. **`threshold` in `tasks/*/tests/fixtures/visual-suite.json`**: every visual checkpoint declares `"threshold": 0.99`, required by authoring. The scorer in `tasks/*/tests/websitebench/harbor/case_protocol.py` (line 55) defines `VISUAL_SSIM_PASS: float = 0.99` and uses it directly — `item["ssim"] >= VISUAL_SSIM_PASS` — never reading the checkpoint's threshold field.

2. **`budgets` in `tasks/*/tests/evaluation-contract.json`**: every contract declares a full budgets block with five subfields. Only `build_timeout_sec` is read in `tasks/*/tests/run.py`; the other four (`cpus`, `memory_mb`, `startup_timeout_sec`, `storage_mb`) are defined in every contract but never read.

3. **Two broken docstrings in `case_protocol.py` (line 13)**: the module docstring says `"See docs/scoring-protocol.md and docs/case-design.md"`. Neither file exists under `docs/`.

**Toolkit**

- Tool: I read the issue body, then cloned the repo, read `case_protocol.py`, `evaluation-contract.json`, `visual-suite.json`, and `run.py` directly, and ran grep to confirm each finding
- What it produced: exact file paths and line numbers for all three problems; confirmed all three match the issue description
- Difficulties: `case_protocol.py` is a large shared file copied into every task directory; the fix needs to be made to a canonical source and propagated, not applied task by task
- What changed: I will raise the propagation question with the maintainer along with the remove-vs-enforce question

---

## Reproduction Process

**Environment Setup**

macOS 15.6.0 (Darwin 25.6.0), Apple Silicon, Python 3.13. Forked and cloned `TIGER-AI-Lab/WebsiteBench` to `~/WebsiteBench` (commit `3868d82`, main branch as of 2026-11-17). No install was needed for the reproduction — all three findings come from reading the source and running grep. The repo requires Python ≥3.12 and pins `harbor==0.25.0`; I did not run the full test suite since it requires Harbor containers that are not publicly available. The repo has no CONTRIBUTING.md.

**Steps to Reproduce**

1. Clone the repo: `git clone https://github.com/TIGER-AI-Lab/WebsiteBench`
2. Run from the repo root to confirm `threshold` is never read during scoring:
   ```bash
   grep -r "VISUAL_SSIM_PASS" tasks/apple/tests/websitebench/harbor/case_protocol.py
   # → VISUAL_SSIM_PASS: float = 0.99
   # → item["ssim"] >= VISUAL_SSIM_PASS for item in result["visuals"]
   grep -rn '"threshold"' tasks/apple/tests/websitebench/harbor/case_protocol.py
   # → no hits
   ```
3. Run to confirm only `build_timeout_sec` is read from the budgets block:
   ```bash
   grep -rn "budgets" tasks/apple/tests/run.py
   # → contract["budgets"].get("build_timeout_sec", 900)
   ```
4. Run to confirm the two documentation files cited in the docstring do not exist:
   ```bash
   ls docs/
   # → README.md  cloud.md  development.md  pypi.md  running.md  task-format.md  tasks.md
   # scoring-protocol.md and case-design.md are absent
   ```

**Reproduction Evidence**

- **Commit showing reproduction:** no code change yet — the reproduction is a read-only source inspection; the commit will come with the fix in Unit 7
- **Screenshots/logs:** not applicable — grep output is shown inline in steps 2–4 above
- **My findings:** all three findings confirmed. `threshold` in visual-suite.json checkpoints is never read; the scorer uses the hardcoded constant `VISUAL_SSIM_PASS = 0.99` instead. Four of the five `budgets` subfields in evaluation-contract.json are unused — only `build_timeout_sec` is read. The `case_protocol.py` module docstring cites `docs/scoring-protocol.md` and `docs/case-design.md`, neither of which exists.

**Toolkit**

- Tool: repro-check in live mode, grading claim-draft.md and repro-draft.md together against the issue URL
- What it produced: verdict `accept` on all required checks; flagged one marginal note: my claim comment was posted before the exploration and therefore promises future work I had already done
- Difficulties: the scorer is a propagated copy of `case_protocol.py` across every task directory; there is no single canonical source file in the main `src/` tree, which means any fix needs a propagation strategy rather than a single-file edit
- What changed: I added the propagation question to what I will raise with the maintainer

---

## Solution Approach

**Analysis**

After reading the source, the root cause is now concrete:

- **`threshold`**: the visual scorer (`case_protocol.py` line 55, `VISUAL_SSIM_PASS = 0.99`) hardcodes the pass threshold as a constant. The checkpoint objects in `visual-suite.json` require a `threshold` field for schema compliance, but the scorer never reads it. The two values happen to match (0.99), which is why the problem has been invisible in production.
- **`budgets`**: `tasks/*/tests/run.py` only reads `build_timeout_sec`; the other four budget fields (`cpus`, `memory_mb`, `startup_timeout_sec`, `storage_mb`) are present in every contract but have no code that acts on them.
- **Broken docstrings**: the `case_protocol.py` module docstring (line 13) cites two files that don't exist.

A new complication: `case_protocol.py` is a propagated copy inside every task directory (`tasks/*/tests/websitebench/harbor/case_protocol.py`), not a single source file. Any fix needs to go into the canonical source that the propagation script reads, then be re-propagated. I need to ask the maintainer where the canonical source is and how propagation works before I write any code.

**Proposed Solution**

Still leaning toward removing unused fields rather than enforcing them, because removing dead configuration is simpler, leaves no ambiguity, and is easier to review. For the docstrings, I will replace the two dead references with an accurate description of what `case_protocol.py` does (or remove the sentence entirely if the maintainer prefers). I will not touch the SSIM scoring logic or the `build_timeout_sec` reader — those are in scope of the issue only if the maintainer decides to enforce thresholds. I will not touch anything beyond what the issue explicitly describes.

**Implementation Plan**

1. Post a follow-up comment on the issue asking: (a) remove vs. enforce, (b) where the canonical `case_protocol.py` source is and how propagation works, (c) how to run the test suite (the contracts require Harbor containers)
2. Wait for a response before writing any code
3. Once direction is received: edit the canonical source, run the propagation step, update `visual-suite.json` schema (or the scorer) per the decision
4. Fix the two broken docstrings in `case_protocol.py`
5. Run `ruff check src/ scripts/` and `ruff format --check src/ scripts/` to pass linting
6. Run whatever test command the maintainer points to

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

**2026-11-17 (Unit 6)**

Installed voice-guard. Patched the rubric scope check from "Bounded scope" to "scope-feasible" — open design edges now raise a difficulty note instead of failing the verdict. Re-graded both picks; WebsiteBench#24 is `accept`, difficulty `moderate`. Forked and cloned the repo. Confirmed no CONTRIBUTING.md exists. Wrote `CLAUDE.local.md` with the inferred conventions, the exact subsystem files, and the ruff commands. Ran repro: all three findings confirmed by reading the source. Repro-check accepted the package.

New finding from the repro: `case_protocol.py` is a propagated copy inside each task directory, not a single source file in `src/`. This means the fix needs to go through the propagation pipeline. I need to ask the maintainer about this before writing any code, in addition to the remove-vs-enforce question. Will add a comment to the issue thread once I know what I am asking.

**[Date to be filled]**

Will be filled in as I build and test the fix in Unit 7.

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

From Unit 5: a rubric that checks whether a repo is "in use" needs a community-adoption floor, not just activity recency. A solo owner can push 10 commits today and pass every activity check while the repo has zero other contributors.

From Unit 6: reading a real production codebase to find where a behavior lives takes a different mindset from reading a classroom repo. The `case_protocol.py` file is 4800 lines long and is not in `src/` — it's a propagated copy bundled into each task directory. The issue description pointed me at the right files, but it took actual source reading to understand the propagation pattern and realize the fix is not a simple single-file edit. I also learned that "confirm the issue is real" sometimes means grep and source-reading rather than actually running the failing code, especially when the runtime requires infrastructure (Docker containers, Harbor) that is not straightforward to set up locally.

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
- `tasks/apple/tests/websitebench/harbor/case_protocol.py` — the shared scoring module where `VISUAL_SSIM_PASS` is defined and the broken docstrings live
- `tasks/apple/tests/evaluation-contract.json` — the contract schema showing the unused `budgets` subfields
- `tasks/fandango/tests/fixtures/visual-suite.json` — a visual suite showing the unused `threshold` field in each checkpoint
