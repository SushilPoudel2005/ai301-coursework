# Evidence guide: where evidence lives in a plan package

In an eval bundle the parts are: **Repo facts** (contribution policy, bug-report template asks), **Issue** (title, body, labels), **Thread highlights**, **Repro evidence** (the accepted reproduction: steps, outputs, Expected, Actual), **Candidate plan** (Diagnosis, Scope, Files, Approach, Test plan, any risks), and **Candidate plan comment**. Live, the issue and thread are the GitHub page, the repro evidence is the student's posted repro comment (house issue: the repro pack as quoted in the drafts), the repo docs are `CONTRIBUTING.md` and templates, and the plan and comment are the student's drafts only.

## Diagnosis and grounding

- Where it lives: the plan's Diagnosis section; the repro evidence's outputs, control runs, and Expected/Actual lines.
- What good looks like: the stated cause accounts for every observation, including the control (what works and what fails), and cites behavior the repro actually shows. A diagnosis that contradicts an observation, dismisses a variable the repro shows matters, or names a cause with no support in the repro is not grounded.

## Scope

- Where it lives: the plan's Scope section (in scope, not in scope) and its Files list.
- What good looks like: one named fix, an explicit out-of-scope line, and files that follow from the diagnosis. A drive-by rewrite adds refactors, cleanup, other issues, or "whatever else is needed".

## Executability

- Where it lives: the plan's Files and Approach (and the order of the steps).
- What good looks like: files or areas named with what changes in each, and a mechanism a stranger could implement. "Improve the handling" or an unnamed file is a wish, not a step.

## Test plan

- Where it lives: the plan's Test plan, set beside the repro evidence's steps and Expected line.
- What good looks like: re-runs the reproduction (or a check through the real code) and states the specific result expected after the fix, matching the repro's Expected. A vague test plan says "run tests" or names no observable outcome.

## Honesty

- Where it lives: the plan's risks/unknowns section, the confidence of its claims, and (after a build) its Deviations section.
- What good looks like: open unknowns stated as unknowns; claims no firmer than the repro supports. Red flags: a guess worded as fact, "this will definitely fix it", an unknown the repro leaves open and the plan does not mention.

## Comms

- Where it lives: the plan comment, set beside the thread highlights and the Repo facts block (contribution policy, templates, stated process).
- What good looks like: the comment says what plan.md says, answers the thread (a maintainer's direction, an open PR or claim, a process rule such as approved-issues-only or discuss-first), and meets the repo's AI-use and own-words rules, including disclosure when required. Boilerplate that would fit any issue is not thread-aware.
