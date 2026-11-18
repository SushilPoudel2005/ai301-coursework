# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Student:** Sushil Poudel

## Featured Tools

I chose these four tools because they represent the complete operational lifecycle of my work in open source this term: discovering issues, guarding public communication, verifying code quality before shipping, and tracking pull requests through maintainer review. Each tool gave me a structured way to separate automated checks from my own judgment.

**issue-scout**

**What it is, and the judgment hole I wrote:** An automated scouting tool that searches GitHub for open-source issues aligned with my technical interests and grades them against an acceptance rubric. I wrote the six-query search strategy covering evaluation frameworks, RAG, and backend data work, along with the multi-factor rubric that grades maintainer responsiveness, activity recency, and repo adoption.

**How it evolved, dated:** On 2026-11-17, during Unit 5, my initial scan returned 536 results and ranked five candidates, but every single one came from brand-new repositories created in the preceding week (one created just ten minutes prior). The original "Repo in use" check passed on commit recency alone without verifying community adoption. I patched the rubric to require at least 20 stars or at least two distinct human authors in the last five commits. The second scan immediately rejected the solo repositories and surfaced WebsiteBench#24.

**Evidence:** Rubric before/after comparison from my Unit 5 patch:

```markdown
Before:
| Repo in use | Not archived AND (latest release within 365 days OR last push within 90 days). |

After:
| Repo in use | Not archived AND (latest release within 365 days OR last push within 90 days) AND (star count >= 20 OR at least 2 distinct human authors in the last 5 commits). |
```

Scan 2 output excerpt:
```text
CloudCaptain#199: docs/curriculum (weak fit)
WebsiteBench#24: Python evaluation harness, 511 stars (strong fit, accept)
Regression suite score post-patch: 19/20 passed.
```

**voice-guard**

**What it is, and the judgment hole I wrote:** A pre-tool enforcement hook that executes before any `gh issue` or `gh pr` command to review public comments before they are posted upstream. I wrote the Upstream section of `voice-guide.md`, specifying rules for making verifiable claims, asking clarifying questions, and managing maintainer pings.

**How it evolved, dated:** On 2026-11-17, during Unit 6, my reproduction check flagged that my draft claim comment promised future implementation work using misleading phrasing for code I had already inspected. I updated `voice-guide.md` to forbid speculative promises and require claim comments to focus strictly on clarifying architectural direction when open design choices exist.

**Evidence:** From `~/.claude/voice-guard/voice-guide.md` Upstream rules:
```text
Claim comments: State the observed symptom and reproduction basis directly. If design choices exist (e.g. remove vs. enforce), ask maintainer direction before promising a specific implementation. Never promise timelines or work outside the confirmed bug scope.
```
Posted claim on WebsiteBench #24 adhering to the guide:
`https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24#issuecomment-6963901597`

**ship-gate**

**What it is, and the judgment hole I wrote:** A pre-flight verification gate that validates repository readiness before pull requests are opened. I authored `ship-manifest.md`, defining the exact sequence of required project checks (`uv sync`, `ruff check`, `ruff format --check`, and `python scripts/check_tasks.py --code-only`), along with the pull request description contract.

**How it evolved, dated:** On 2026-11-17, during Unit 7, an API spend limit interrupted the automated CLI precheck hook during pre-flight. Instead of skipping checks, I transitioned the process into an explicit manual CLI execution protocol using absolute paths for the PR body draft and running each gate in order against all 70 task suites.

**Evidence:** Terminal verification output from running the gate commands:
```bash
$ uv run ruff check
All checks passed!
$ uv run ruff format --check
70 files already formatted.
$ python scripts/check_tasks.py --code-only
Checking 70 tasks...
All tasks passed code validation.
```
Resulted in upstream pull request: `https://github.com/TIGER-AI-Lab/WebsiteBench/pull/94`

**pr-monitor**

**What it is, and the judgment hole I wrote:** A tracking skill that inspects upstream pull request threads, monitors GitHub Actions and CI status, and flags maintainer comments. I authored the response playbook defining staleness windows, review response guidelines, and walk-away criteria.

**How it evolved, dated:** On 2026-11-18, during Unit 8, I set up the tracking configuration for open pull request #94 on WebsiteBench. The playbook established a 14-day threshold before any polite follow-up nudge, preventing premature maintainer pings while the maintainers consider the remove-vs-enforce question.

**Evidence:** Log excerpt from `monitor-log.md`:
```text
PR #94: State=OPEN, Mergeable=CLEAN, CI=PASSED
Review status: Awaiting maintainer feedback on remove-vs-enforce schema question.
Action: No ping required; staleness counter 1/14 days.
```

## Inventory

- repro-check: Source inspection and diagnostic skill with a rubric for proving bugs before writing fixes; used to verify WebsiteBench#24 grep evidence.
- plan-check: Pre-implementation validation skill that checks issue scope, test strategy, and boundary conditions before code changes begin.

## The Arc

**Beat 1**
Date: 2026-11-17
Friction: The first issue-scout scan returned five candidate issues, but every single one belonged to a solo repository created in the last week because the rubric only checked commit recency.
Change: Patched the "Repo in use" check in rubric.md to require at least 20 stars or at least two distinct human authors in the last five default-branch commits.
Result: The second scan immediately rejected the solo repositories and surfaced WebsiteBench#24 as a high-value match with 511 stars.

**Beat 2**
Date: 2026-11-17
Friction: Repro-check flagged that my draft claim comment used future-tense promises for work already explored, while code inspection showed scoring logic was duplicated across task folders rather than centralized.
Change: Updated voice-guide.md to forbid speculative implementation promises and rewritten the claim comment to ask maintainers whether they preferred removing or enforcing contract fields.
Result: Posted claim comment cleanly engaged maintainers without prematurely committing to a breaking change.

**Beat 3**
Date: 2026-11-17
Friction: An API spend limit interrupted the automated CLI precheck hook right before submitting the WebsiteBench pull request.
Change: Switched to manual execution of the ship-manifest verification steps, running ruff and task integrity checks via CLI with absolute paths.
Result: Successfully opened upstream pull request #94 with all 70 task suites validated and zero lint errors.

**Beat 4**
Date: 2026-11-18
Friction: PR #94 remained open with no immediate maintainer response, risking either premature follow-up pings or abandoning tracking.
Change: Implemented the PR-monitor response playbook setting a 14-day quiet window and specific walk-away conditions.
Result: Pull request status and CI health are tracked cleanly without unnecessary maintainer interruptions.

## Limitations and What's Next

The toolkit provided reliable guardrails across discovery, verification, and monitoring, but it still has architectural gaps. The scout depends on keyword matching that brings in noisy results on generic terms, while the verification gates depend on local static checks rather than running full containerized integration tests.

For my next iterations, I will integrate star filters directly into the GitHub search query syntax to save scan quota, and build Docker/Harbor execution fixtures into ship-gate so containerized benchmarks can run end-to-end before PR submission.

**Tool limits.**
issue-scout: The `prompt` keyword query returns documentation and UI noise, and adoption filtering occurs after ranking rather than in the initial search query.
voice-guard: Flags disallowed phrases and tone patterns with heuristics, but cannot verify whether technical claims in a PR description match the actual git diff.
ship-gate: Relies on static code and task checks because full integration benchmark evaluation requires proprietary Harbor container images that cannot run in this environment.
pr-monitor: Tracks thread status and comment timestamps, but cannot interpret the semantic content of maintainer review feedback to generate draft code revisions.
