# Rubric: is this pull request ready to submit?

Every check reads the thing itself (the diff, the evidence, the description) against the plan, the issue and the repo's stated standards, never the length or format of the write-up. A PR that honestly discloses a shortfall can be ready.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | The diff's changed files and hunks read against the plan's scope pair (in scope / not in scope), its Files list and its Deviations notes; the description's claims read against the diff | Every changed file and behavior falls inside the plan's stated scope or is covered by a deviation note, and nothing the plan promised is silently missing. Anything the diff does beyond or short of the plan is disclosed in the description or a deviation note. The description claims no more and no less than the diff delivers. It fails if the diff silently does more or less than the plan, or the description claims something the diff does not contain. | required |
| Tested | The test evidence's before and after, read against the plan's test plan and the issue's repro steps; the outcome of the repo's own checks | The evidence shows an observable behavior from the repro or test plan, before and after, with the expected-after result stated and matching what the diff should produce, AND the outcome of the repo's own checks or suite is shown (or an honest reason is given that one could not be run). It fails if the evidence is only "tests pass", has no before state, tests something other than the planned behavior, or the repo's own checks were never run and no reason is given. | required |
| Reviewable diff | The unified diff and the commit list | The fix is visible and nothing unrelated rides along. It fails if the diff contains debug leftovers, dead or commented-out code, formatting churn on lines the fix does not touch, unrelated hunks or files, or committed planning files, such that a reviewer could not tell which lines are the fix. | required |
| Standards and disclosure | The description read against the repo-facts' PR template asks, contribution policy and stated process; the thread's maintainer direction | Every section the repo's PR template asks for has real content for this change (not left as the template's placeholder), the issue link or close keyword the template asks for is present and names this issue, any required AI-use disclosure is present, and explicit maintainer direction in the thread is followed or engaged. It fails if a required section is empty or boilerplate, a required disclosure is missing, or a stated ask is visibly ignored. | required |
| Tight description | The PR title and description | The title says what changed and the description says why, the change and the test in a few sentences. | preferred |

## Verdict rule

Accept (ready to submit) only if every required check passes. Reject (hold) if any required check fails. `unclear` on a required check counts as fail. Preferred checks never change the verdict.
