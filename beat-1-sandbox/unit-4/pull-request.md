# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/255

**Branch**

`test/12-prompt-template-snapshots`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 20/20, every category matched (`clear-accept 7/7`, `not-tested 4/4`, `silent-drift 4/4`, `standards-wall 2/2`, `unreviewable 3/3`). I had not passed `--save-run`, so this one is not the saved file.
2. Full run with `--save-run`, same unchanged tool: 20/20, every category matched. This is the run in `eval-run.txt`.

Last score: 20/20.

**Package analysis**

pkg-01 (pandas, `read_csv` C engine heap overflow, category `standards-wall`). My tool decided **reject** and the gold label is **reject**. It failed two required checks. On "Plan fidelity": the description says it "adds a capacity check to the bulk-copy path", but the diff puts the check at the top of `tokenize_bytes` before the loop, duplicating the existing `make_stream_space` call, while the plan called for checks in the bulk-copy paths. On "Standards and disclosure": the repo's PR template requires "closes #xxxx with the issue number" and a whatsnew entry for a bug fix, and the description never references #66657 and the diff has no whatsnew entry. The rubric reads the description against the repo-facts template asks and the diff against the plan, so it caught both a claim the diff does not match and a stated ask that was ignored. The "Plan fidelity" check exists to catch the first kind, and "Standards and disclosure" the second.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`, exactly as it reads now:

| Standards and disclosure | The description read against the repo-facts' PR template asks, contribution policy and stated process; the thread's maintainer direction | Every section the repo's PR template asks for has real content for this change (not left as the template's placeholder), the issue link or close keyword the template asks for is present and names this issue, any required AI-use disclosure is present, and explicit maintainer direction in the thread is followed or engaged. It fails if a required section is empty or boilerplate, a required disclosure is missing, or a stated ask is visibly ignored. | required |

This check reads the description against the repo's own template asks, the way the repo states them in the repo-facts block, instead of a generic idea of a good description. I named the three things that sank pkg-01 and pkg-20 in the set: a template section left empty, a missing issue-closing line, and a missing AI-use disclosure the policy requires. I kept the claim-matches-diff question out of it and put it under Plan fidelity, because the evidence guide treats a description that claims more or less than the diff as drift, not a comms problem. I rejected a version that graded the length or polish of the description, because the rubric guidance says to judge whether the stated asks were met, not the shape of the write-up.

**Trade-offs**

Nothing changed between my two full runs: the same tool, with no edits to the rubric, procedure, evidence guide or frame, agreed on 20/20 both times with all five categories matched, so I have no flipped package to report. What the quoted check gives up is leniency on process asks. It treats a repo's stated asks as required, so a PR with a correct fix can be held for a missed ask. I saw this on my own PR: the live run held it because my only commit message did not follow the Conventional Commits format that `docs/CONTRIBUTING.md` requires, although the diff, the tests and the description all passed. I accept that cost because a stated ask the repo makes is part of what a maintainer will bounce a PR for.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
