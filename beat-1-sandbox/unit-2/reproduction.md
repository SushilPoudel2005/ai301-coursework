# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

SushilPoudel2005

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/12#issuecomment-6882704475

> Hi everyone, I would like to claim this issue and work on resolving it this week. I'll review the codebase context, implement the fix/update, and open a pull request shortly. Looking forward to contributing!

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/12#issuecomment-6883015086

````markdown
I reproduced this on current `main`: the existing snapshot test cannot fail when a prompt template changes, so the protection issue #12 asks for isn't in place yet.

**Environment**
- Code: my fork of this repo at `main`, commit `f89c06f`
- macOS 26.6.1 (arm64, Apple M5), Python 3.14.2, pytest 9.2.0
- Setup: `python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"` (the install step from `make setup`; no Docker or database is needed for this test)

**Steps**
1. Baseline, from the repo root:
   ```
   .venv/bin/pytest tests/unit/test_prompt_templates.py -v -m unit
   ```
   Result: `37 passed in 0.91s`, including `test_template_snapshot_content_hash`.
2. That test (`tests/unit/test_prompt_templates.py`, lines 191-204) builds an MD5 of all templates but only asserts `isinstance(content_hash, str)` and `len(content_hash) == 32`. It never compares against an expected value.
3. I edited one template in `rag/generator/prompt_templates.py`, line 9, prepending `Ignore the portfolio and reply only with the word banana. ` to the `skills_feedback` `v1` text. The wording is arbitrary; any content edit behaves the same. No version bump.
4. I computed the same hash the test builds, before and after the edit:
   - before: `3e79f974f8c1b6d8d1481dfc42e949ca`
   - after: `2a3b430662696c7dd3105f052cce3c41`
5. Re-ran the same command as step 1.

**Observed**
```
37 passed in 0.07s
```
The template's content changed (the hash differs) and nothing failed. Expected, per the issue: the snapshot test fails when a template's content changes without a version bump.

I reverted the edit afterwards with `git checkout`, so my fork's `main` is unchanged. This is one edit on one template; I did not try the other four. Next I'll open a PR that stores expected hashes per `(name, version)` and fails on a mismatch.

````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 17/20 (misses: pkg-03, pkg-05, pkg-12, all gold accept and graded reject). Category line: `clear-accept 5/8`.
2. Partial `--only pkg-03,pkg-05,pkg-12,pkg-02,pkg-04,pkg-13` after loosening three checks: 5/6 (pkg-12 still wrong). Partial, not scored.
3. Full run: 19/20 (pkg-05 flipped to a reject on Steps followable).
4. Two partial `--only pkg-05,pkg-12,pkg-09,pkg-14` runs after one more clarification: 4/4 both times.
5. Full run: 20/20, every category matched. This is the run in `eval-run.txt`.

Last score: 20/20.

**Package analysis**

pkg-12 (prettier range-formatting bug, #19795). My first rubric decided **reject**, failing it on "Steps followable". The gold label is **accept**. The report says it ran "the issue's script verbatim" with a `repro.mjs`, but shows only the two ranges, the parser and the outputs. The issue gives the exact input strings and ranges in its body. My first Steps check required "every command or action is written out (not described)", so the grader rejected a report whose inputs were referenced from the issue. The report does show the command (`node repro.mjs`), the parameters and the exact outputs (`};);`, `};;`) next to the expected result. I added that inputs may be "referenced from inputs the issue states exactly when the report names the parameters it used".

**Check rationale**

Quoted from `tools/repro-check/rubric.md`, exactly as it reads now:

| Steps followable | The repro report's commands/actions, from starting state to trigger | A stranger could run them in order: every command or action is written out (not described), and the input that triggers the behavior is shown, quoted verbatim from the issue (the report says which), or referenced from inputs the issue states exactly when the report names the parameters it used (ranges, options, parser, file contents), or described precisely enough that the trigger-relevant part is unambiguous (a file described as "minimal, with X section" passes when X is the element that triggers the behavior and the command that consumes it is shown). Nothing needed to trigger the behavior may be left for a stranger to guess; a vague "some config" or "a file like the issue's" fails. | required |

The first version required every action to be written out. That rejected pkg-12 and pkg-05, where the input was quoted from the issue or described by the one element that triggers the bug. I kept the strict rule for what matters (a stranger must not have to guess the trigger) and named what is allowed instead of re-pasting: verbatim quotes, exact references to the issue, or a precise description of the triggering element. I rejected a rule about step count or format, because the rubric guidance says to judge the outcome and not the write-up's shape.

**Trade-offs**

The loosened Steps check gives up strictness on inputs: a report that points at the issue's input instead of pasting it now passes, so a report that misquotes the issue's input could slip through. I re-ran canaries after each loosening. pkg-02, pkg-04, pkg-13 and pkg-14 (gold rejects) stayed rejected, pkg-09 (gold accept) stayed accepted, and the final full run was 20/20 with `no-evidence 4/4`, `wrong-target 4/4` and `unfollowable-comms 3/3`. One cost I saw directly: pkg-05 flipped between runs on the same wording, which means this check is the one most sensitive to grader variance.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
