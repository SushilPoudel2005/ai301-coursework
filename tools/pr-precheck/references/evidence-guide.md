# Evidence guide: where evidence lives in a PR package

In an eval bundle the parts are: **Repo facts** (PR template asks, contribution policy), **Issue**, **Thread highlights**, **Plan context** (the repro evidence, the plan's scope pair, Files, Test plan, any deviation notes), and the **Candidate PR** (Title, Description, Commits, Diff, Test evidence). Live, the plan is `plan.md` with its Deviations section, the diff is `git diff main...HEAD`, the commits are `git log main..HEAD --oneline`, the evidence is the student's saved before/after output and check results, the draft title and description are the student's drafts, and the repo's asks are `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.

## Plan fidelity (harness category: silent-drift)

- Where it lives: the plan's scope pair, Files list and Deviations notes; the diff's changed files and hunks; the description's claims about what the PR does.
- What good looks like: every changed file and behavior sits inside the plan's stated scope or a deviation note, and nothing the plan promised is missing without a note. Silent drift is more than the plan (an extra file, a rewrite, an unplanned behavior change) or less than the plan with no note. A description that claims more or less than the diff delivers is drift, not a comms problem.

## Test evidence (harness category: not-tested)

- Where it lives: the Test evidence section (or the student's saved output), the plan's Test plan, the issue's repro steps, and the outcome of the repo's own checks.
- What good looks like: a named observable behavior shown before and after, with the expected-after result stated and matching the diff's intended effect, and the checks' outcome visible (or an honest reason one could not be run). "Tests pass" alone, no before state, or evidence about something other than the planned behavior is not decisive.

## Diff quality (harness category: unreviewable)

- Where it lives: the unified diff and the commit list.
- What good looks like: the fix is visible and nothing unrelated rides along. Debris tells: debug prints, dead code, commented-out blocks, formatting churn on untouched lines, drive-by edits, unrelated files, committed scratch or planning files.

## Standards and comms (harness category: standards-wall)

- Where it lives: the repo-facts block's PR template sections, contributing instructions and AI policy; the description; the thread's maintainer direction.
- What good looks like: every section of the template has real content for this change, the issue-closing line names the right issue, any required AI-use disclosure is present, and explicit maintainer direction in the thread is followed or engaged. A template left as placeholders or an ignored stated ask is a miss. (Whether the description's claims match the diff is plan fidelity.)
