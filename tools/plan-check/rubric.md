# Rubric: is this plan ready to post and build from?

Every check reads the thing itself against the issue and its repro evidence, never the shape of the write-up.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis grounded | The plan's Diagnosis, read against the repro evidence's steps, outputs and stated Actual/Expected | The stated cause is consistent with every observation in the repro evidence (including the control runs) and explains the behavior shown. It fails if it contradicts an observation (for example, calls a variable a red herring when the repro shows it changes the result), ignores a decisive observation, or asserts a cause the repro evidence gives no support for. | required |
| Fixes the cause | The plan's Approach, read against the cause the repro evidence points at | The change targets the cause the evidence points at, not only the visible symptom. It fails if the change would hide or work around the symptom (suppress the error, special-case the failing input, adjust the test) while the cause the evidence points at stays in place. | required |
| Bounded scope | The plan's Scope statement (in/out) and Files list, read against the Diagnosis | One bounded change: the in-scope statement names a single fix, and a not-in-scope line (or equivalent) excludes the rest. The listed files follow from the diagnosis. It fails if it bundles unrelated work (refactors, cleanup, new features, other issues), touches areas the diagnosis does not require, or leaves the boundary open ("and anything else needed"). | required |
| Executable | The plan's Files, Approach and order of work | A stranger could start building without asking the author anything: the files or areas are named, the approach says what changes in each, and the order is clear. It fails if steps are intentions ("improve handling", "make it robust") or key files or the mechanism are left unnamed. | required |
| Decisive test plan | The plan's Test plan, read against the repro evidence's steps and Expected result | The test plan re-runs the reproduction (or a check through the real code) and states the specific observable result expected after the fix, matching the repro's Expected. It fails if it only says "run the tests", names no expected observable outcome, or tests something other than the reproduced behavior. | required |
| Honest about unknowns | The plan's Risks/unknowns (or equivalent) and the certainty of its claims | Unknowns and risks the evidence leaves open are stated as unknowns, and no claim is stated more firmly than the repro evidence supports. A claim counts as supported when the repro evidence or a maintainer statement in the thread backs it, and a plan that names its main risk passes without listing every conceivable unknown. It fails if a guess with no support in the repro or thread is presented as established fact, or a decisive unknown the repro leaves open (one the fix depends on) is hidden. | required |
| Comment thread-aware and conventional | The plan comment, read against the thread highlights and repo-facts (contribution policy, templates, bug-report asks) | The comment states the plan the plan.md contains (no claim the plan does not make), responds to what the thread says (a maintainer's direction, a request, an already-open PR or claim), and respects the repo's stated conventions: any required AI-use disclosure is present, the policy's own-words or human-written asks are met, and any stated process (such as an approved-issue or discuss-first rule) is followed. A boilerplate comment that would fit any issue fails. | required |
| Tight comment | The plan comment | Short, says the cause, the change, and the test in a few sentences, with no filler. | preferred |

## Verdict rule

Accept (ready to post and build from) only if every required check passes. Reject (hold) if any required check fails. `unclear` on a required check counts as fail. Preferred checks never change the verdict.
