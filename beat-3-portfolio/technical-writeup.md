# Patching Silent Contract Bypasses Across 70 Evaluation Suites in WebsiteBench

## Overview

In evaluating generative web agents, benchmark benchmarks rely on strict evaluation contracts to guarantee reproducible testing. In [TIGER-AI-Lab/WebsiteBench](https://github.com/TIGER-AI-Lab/WebsiteBench), however, [Issue #24](https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24) revealed that critical evaluation-contract fields—including visual checkpoint `threshold` values and container `budgets`—were silently ignored by the test scorer in favor of hardcoded constants, while module docstrings referenced nonexistent design documentation. To find and vet this issue, I built an automated evaluation skill called `issue-scout`, writing my own six-query search strategy and a multi-criteria evaluation rubric. After diagnosing the root cause and discovering that the scoring logic was replicated across 70 separate task suites rather than centralized in a single module, I wrote an automated Python refactor script to synchronize docstrings, document the unread fields, and validate all 70 test suites. Today, my fix is open upstream in [WebsiteBench PR #94](https://github.com/TIGER-AI-Lab/WebsiteBench/pull/94), passing all formatting and task integrity checks while awaiting final maintainer direction on schema enforcement.

## What happened

### 1. Scouting the issue and refining the rubric

To identify high-impact contributions in AI evaluation frameworks, I developed `issue-scout` using a custom six-query search strategy targeting evaluation harnesses and backend data systems. My first automated scan returned 536 results, ranking five top candidates. However, inspection revealed an immediate defect in my vetting logic: every single accepted candidate came from a solo repository created in the preceding seven days, including one repository that had been pushed ten minutes prior. 

The original "Repo in use" check passed any repository with commits in the last 90 days, failing to verify genuine community adoption or external maintainer review. To fix this, I patched the rubric to introduce a strict community-adoption floor:

```markdown
| Check | Before Patch | After Patch |
|---|---|---|
| Repo in use | Not archived AND (latest release within 365 days OR last push within 90 days). | Not archived AND (latest release within 365 days OR last push within 90 days) AND (star count >= 20 OR at least 2 distinct human authors in the last 5 commits). |
```

I re-ran my regression test suite against this patched rubric, achieving a 19/20 pass rate. Executing a second scan immediately filtered out the solo projects and surfaced [WebsiteBench Issue #24](https://github.com/TIGER-AI-Lab/WebsiteBench/issues/24) as the top match: an active project with 511 stars, responsive maintainers, and an unresolved bug in evaluation-contract processing.

### 2. Confirming the bug through code inspection

After claiming the issue upstream, I cloned the repository and investigated the evaluation pipeline. The issue reported that `threshold` fields in `visual-suite.json` and several `budgets` fields in `evaluation-contract.json` were never read during verification. 

Running search queries across the task suites confirmed the exact disconnect. In `case_protocol.py`, the scorer never parsed `threshold` from checkpoint payloads; instead, it evaluated image similarity against an internal static constant:

```bash
$ grep -rn "VISUAL_SSIM_PASS" tasks/apple/tests/websitebench/harbor/case_protocol.py
55: VISUAL_SSIM_PASS: float = 0.99
282: return item["ssim"] >= VISUAL_SSIM_PASS

$ grep -rn '"threshold"' tasks/apple/tests/websitebench/harbor/case_protocol.py
# (no matches found)
```

Similarly, examining the budget configuration in `tasks/apple/tests/run.py` revealed that only `build_timeout_sec` was read, leaving `cpus`, `memory_mb`, `startup_timeout_sec`, and `storage_mb` completely dormant:

```python
# tasks/apple/tests/run.py
build_timeout = contract["budgets"].get("build_timeout_sec", 900)
# Other budget fields defined in schema are never referenced
```

Finally, checking the documentation references cited in the header of `case_protocol.py` showed that both cited files were missing:

```bash
$ ls docs/
README.md  cloud.md  development.md  pypi.md  running.md  task-format.md  tasks.md
# docs/scoring-protocol.md and docs/case-design.md are absent
```

### 3. Adapting implementation across distributed task suites

My initial plan assumed `case_protocol.py` was a centralized library in `src/` where a single edit would fix the issue. Code inspection revealed a much more complex architecture: `case_protocol.py` was not imported from a central package, but was independently duplicated inside all 70 distinct task directories (`tasks/*/tests/websitebench/harbor/case_protocol.py`).

Furthermore, removing or modifying the contract fields unilaterally could break downstream consumers or alter historical benchmark scores without maintainer agreement. I adapted my approach:
1. I posted a clarification question to the issue thread asking whether maintainers preferred removing unused fields or enforcing them.
2. Rather than editing 70 files by hand, I authored an automated Python script to update the docstrings and annotate the unread threshold constant across every task directory:

```python
# Updated snippet applied across all 70 copies of case_protocol.py
"""Evaluation protocol for WebsiteBench harbor evaluation."""
...
# Note (Issue #24): Per-checkpoint threshold in visual-suite.json is unread by the scorer.
# Visual comparison currently enforces the static VISUAL_SSIM_PASS threshold.
VISUAL_SSIM_PASS: float = 0.99
```

3. I executed local verification commands across the entire workspace to ensure zero formatting or syntax regressions:

```bash
$ uv run ruff check
All checks passed!
$ uv run ruff format --check
70 files already formatted.
$ python scripts/check_tasks.py --code-only
Checking 70 tasks...
All tasks passed code validation.
```

### 4. Landing the pull request upstream

With all local gates passing, I committed the changes to branch `fix/24-sealed-contract-fields` and opened upstream pull request [WebsiteBench PR #94](https://github.com/TIGER-AI-Lab/WebsiteBench/pull/94). The PR details the exact discrepancies found in the scoring logic, documents the changes applied across all 70 task suites, and references Issue #24 so maintainers have a clean, passing foundation from which to decide future contract schema enforcement.

## Limitations and what's next

While [WebsiteBench PR #94](https://github.com/TIGER-AI-Lab/WebsiteBench/pull/94) eliminates dead documentation links, standardizes docstrings, and documents the silent constant override across all 70 test suites, it does not yet alter the evaluation contract schema or enforce runtime budget limits. Enforcing dynamic thresholds (e.g., reading `item["threshold"]` instead of `VISUAL_SSIM_PASS`) or removing fields from `evaluation-contract.json` carries backward-compatibility risks for teams running benchmarks on existing checkpoints. 

Similarly, the `issue-scout` tool still suffers from high noise when querying broad keywords like `prompt`, which often captures instructional text rather than core AI pipelines. Next, I plan to:
1. Restructure `issue-scout` query generation to use compound terms (`prompt template`, `evaluation harness`) and enforce star thresholds directly in GitHub API queries rather than in post-filtering.
2. Once the WebsiteBench maintainers provide guidance on PR #94, submit a follow-up commit to either prune the unused `budgets` schema or dynamically bind per-checkpoint `threshold` values into Harbor test container execution.
