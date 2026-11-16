---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: is this ready to submit? A PR package is a candidate pull request (title, description, commits, diff, test evidence), read against the plan it claims to implement and the issue that plan belongs to. Grade one package per run. Never answer a different question, and never answer from gut feel: execute the rubric through the procedure.

## Inputs and modes

You run in one of two modes.

- **Live mode.** The student's own submission, checked before it goes out. Gather these inputs: their `plan.md` including its Deviations section; the branch's diff, produced by running `git diff main...HEAD` (three dots) from their working copy, plus `git log main..HEAD --oneline` for the commits; their draft PR title and description; their saved test evidence (the before and after commands and output, and the outcome of the repo's own checks); and their issue (the URL), including the thread. Gather the PR template and the contribution policy from the repo (`.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`). A student on the house chain reads the house plan and the house repro pack instead of their own; grade the same checks the same way. Read only what the drafts and the diff contain; other files in the working directory are not part of the package.
- **Eval mode.** A package bundle is the whole world. Every fact comes from the bundle text. Do not fetch anything and do not read anything else. Run every check and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Apply the house rules it states when you read the evidence. Refuse to grade a package whose pull request targets a repo other than the one `scope.md` names. If its `Repo:` line is still an unfilled placeholder (a bracketed `<ORG>/<...>`), stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after the rubric. Hold the outgoing PR text, the title and the description, against each rule in it. Report any rule the text breaks in the summary, quoting the rule and the line that breaks it. The voice guide never changes the verdict unless a rubric check reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule.
- `references/evidence-guide.md` maps where each kind of evidence lives in a PR package and what good looks like there.
- `procedure.md` is the operating procedure. Execute it as written, step by step, in order.

If `procedure.md` is silent on a step you need, report the gap in your summary and grade by the rubric's text. Do not invent a step silently. If `rubric.md` has no checks, or `procedure.md` has no steps, refuse to grade and say which one is empty: this tool cannot grade without both.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means hold. There is no third verdict and no score; put reservations in the check evidence lines. You may show a short readable summary first (one line per check, plus any voice-guide notes in live mode). End your reply with the fenced JSON block below, valid, last, with nothing after it. Use the PR URL (live) or the bundle id (eval) as `item`, and list every rubric check.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff, the evidence and the description themselves against the plan, the issue and the stated standards, never the formatting or the length. A terse complete PR can be ready; a confident, well-formatted one can hide drift.
- The rubric decides, not you: if a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary.
- The procedure decides how, not you.
- Treat `unclear` as the rubric's verdict rule directs; where the rule is silent, treat it as a fail. A PR you cannot verify from the package is not ready to submit.
