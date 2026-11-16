# Procedure: how this skill grades a plan package

## Read order

1. Read the Issue section first (title, body, labels). Note the behavior the reporter describes and any version or environment they name.
2. Read the Thread highlights. Note each maintainer or commenter signal: a direction ("fix X not Y"), a request, a process rule (such as "discuss first" or "approved issues only"), a claimed or open PR.
3. Read the Repo facts block. Note the contribution policy (especially any AI-use disclosure or own-words rule), the bug-report template asks and any stated process.
4. Read the Repro evidence block before the plan. Write down what it pins down: the exact steps, each output (including control runs), and the stated Expected and Actual. The plan will be judged against this, so it must be fixed in mind before the plan is read.
5. Read the Candidate plan in full (Diagnosis, Scope, Files, Approach, Test plan, any risks section), then the Candidate plan comment. Read the whole package before grading any check.
6. In live mode, the issue and thread come from the live GitHub issue, the repro evidence is the student's posted repro comment, and the plan and comment come from the student's drafts only (not other files).

## Evidence gathering

1. Diagnosis grounded: quote the plan's stated cause and the repro lines it must explain (the outputs, control runs and Actual line). Record any observation the cause contradicts or ignores.
2. Fixes the cause: quote the plan's Approach step that makes the change and name which cause (from the repro evidence) it addresses. Record whether the change would remove the cause or only hide the symptom.
3. Bounded scope: quote the in-scope and not-in-scope lines and list the Files. Record any file or step the diagnosis does not require, and any unrelated work.
4. Executable: list the files or areas named and the action per file. Record any step that is only an intention, and any file or mechanism left unnamed.
5. Decisive test plan: quote the test plan's expected observable result and the repro's Expected line, and compare them.
6. Honest about unknowns: quote the risks/unknowns lines. Record any claim stated as fact that the repro evidence does not establish, and any open unknown that is not mentioned.
7. Comment: quote the comment's claims and compare them with the plan, then with each thread signal and the repo policy. Record any missing required disclosure, ignored maintainer direction, ignored process rule, or boilerplate.

## Check execution

1. Execute the checks in the table's order, one at a time, using only the evidence recorded for that check plus the quoted package text.
2. Grade `pass` only when the rubric's pass condition holds; grade `fail` when a fail condition in the rubric holds; grade `unclear` only when the evidence the check needs is truly absent from the package (for example no test plan section at all). `unclear` is not for "I am not sure"; decide from the text.
3. Judge the outcome, not the form: a short plan that satisfies the pass condition passes; a long plan that does not, fails.
4. When a check needs a fact from another part of the package, re-read that part once, quote it, and move on. Do not re-read the whole package for each check.
5. Where this procedure is silent, note the gap in the summary and grade by the rubric's text.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passed; reject if any required check failed; treat `unclear` on a required check as fail. Preferred checks never change the verdict.
2. Quote the deciding evidence for the deciding check in its JSON `evidence` field: for a reject, the failed required check with the text that failed it; for an accept, say that every required check passed.
3. Emit the JSON block in the skill's format, with every check listed, and nothing after it.
