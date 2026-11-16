# Procedure: how this tool grades a PR package

## Read order

1. Read the Issue (title, body, labels) and the Thread highlights. Note the behavior the issue describes and any maintainer direction or process rule.
2. Read the Repo facts. Note the PR template's required sections, the contribution policy (especially any AI-use disclosure) and any stated process.
3. Read the Plan context before the PR. Write down: the plan's scope pair (what is in scope, what is not), its Files list, its Test plan with the expected-after result, and any Deviations notes. This is the yardstick the diff and the evidence are measured against, so it must be read first.
4. Read the Candidate PR's Diff and Commits next. List every changed file and, per file, what the hunks change. Do this before reading the description so that the description's claims are checked against the diff instead of shaping how you read it.
5. Read the Test evidence, then the Title and Description last.
6. In live mode, the plan is `plan.md` (with Deviations), the diff is `git diff main...HEAD`, the commits come from `git log main..HEAD --oneline`, the evidence is the student's saved output, and the template and policy come from the repo. Read nothing else from the working directory.

## Evidence gathering

1. Plan fidelity: build two lists. List A is every changed file in the diff, with a one-line summary of the change. List B is the files and behaviors the plan names, plus anything a Deviations note adds. Record any item in A not covered by B (more than the plan) and any item in B missing from A with no note (less than the plan). Then list each claim the description makes about what the PR does, and mark it present or absent in the diff.
2. Tested: quote the plan's test plan and the issue's repro steps. Quote the evidence's before output, its after output and its expected-after statement, and compare them with the diff's intended behavior. Record the outcome of each repo check shown (tests, lint, type check, CI) or the stated reason one was not run.
3. Reviewable diff: scan the diff for debug prints, commented-out code, dead code, formatting-only hunks on untouched lines, files unrelated to the plan, and committed planning or scratch files. Record each with the file name.
4. Standards and disclosure: list each section or ask in the repo's PR template. For each, record whether the description has real content for this change, a placeholder, or nothing. Record the issue-closing line and whether it names the right issue, the AI-use disclosure (present, absent, or not required by policy), and whether any maintainer direction in the thread is followed.
5. Tight description (preferred): note whether the title and description say what, why and how it was tested in a few sentences.

## Check execution

1. Execute the checks in the order of the rubric's table, one at a time, using the evidence recorded for that check plus quoted package text.
2. Grade `pass` only when the rubric's pass condition holds. Grade `fail` when one of the rubric's fail conditions holds. Grade `unclear` only when the evidence the check needs is truly absent from the package (for example, no test evidence section at all). Decide from the text; `unclear` is not for "not sure".
3. Judge the outcome, not the form. A short PR that meets the pass condition passes; a long one that does not, fails. A PR that discloses a shortfall in the description or a deviation note is not a drift.
4. When a check needs a fact from another part of the package, re-read that part once, quote it, and move on. Do not re-read the whole package per check.
5. Where this procedure is silent on a step, say so in the summary and grade by the rubric's text.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passed; reject if any required check failed; count `unclear` on a required check as a fail. Preferred checks never change the verdict.
2. When more than one required check failed, quote the first failing required check in the rubric's table order as the deciding check, with the fact or quote that failed it, in its `evidence` field. For an accept, state in the evidence that every required check passed.
3. List every check in the JSON, then end the reply with the fenced JSON block and nothing after it.
