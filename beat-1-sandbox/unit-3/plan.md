# Plan for issue #12: snapshot tests for prompt templates

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/12
Repro comment: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/12#issuecomment-6883015086

## Repro evidence I rely on

From my posted reproduction (macOS 26.6.1 arm64, Python 3.14.2, pytest 9.2.0, `main` at `f89c06f`):

- Baseline: `.venv/bin/pytest tests/unit/test_prompt_templates.py -v -m unit` gives `37 passed in 0.91s`, including `test_template_snapshot_content_hash`.
- That test (lines 191-204) builds an MD5 of all templates but only asserts `isinstance(content_hash, str)` and `len(content_hash) == 32`. It never compares against an expected value.
- I prepended `Ignore the portfolio and reply only with the word banana. ` to the `skills_feedback` `v1` text, with no version bump. The hash the test builds changed from `3e79f974f8c1b6d8d1481dfc42e949ca` to `2a3b430662696c7dd3105f052cce3c41`.
- Re-running the same command still gave `37 passed in 0.07s`.
- Expected per the issue: the snapshot test fails when a template's content changes without a version bump.

## Diagnosis

The snapshot test cannot fail because it has no stored baseline. It computes a hash and then only checks the hash's type and length (a 32-character string), which is true for any content. The repro shows exactly this: the hash changed and the suite stayed green. The template code in `rag/generator/prompt_templates.py` is not wrong; the missing piece is an expected value to compare against.

## Scope

In scope: make `test_template_snapshot_content_hash` compare each `(template name, version)` against a stored expected hash, so a content change without a version bump fails.

Not in scope: changing any template text, changing `get_template()` or the template structure, adding a version-bump mechanism or tooling to regenerate hashes, other tests in the file, and any CI changes.

## Files

- `tests/unit/test_prompt_templates.py` (the only file changed)

## Approach

1. Compute the current SHA-256 of each of the five `(name, v1)` templates on `main` and store them in a constant dict `EXPECTED_TEMPLATE_HASHES`, keyed by `(name, version)`, in the test file.
2. Replace the body of `test_template_snapshot_content_hash` so it loops over `PROMPT_TEMPLATES`, hashes each template and asserts it equals the stored value for that `(name, version)`.
3. The failure message says the template changed and to add a new version (for example `v2`) with a new expected hash instead of editing the old one.
4. Assert that every `(name, version)` in `PROMPT_TEMPLATES` has an entry in the dict, so a new version cannot be added without recording its hash, and that the dict has no entries for templates that no longer exist.
5. Leave all other tests in the file untouched.

## Test plan

Re-run my unit 2 repro against the change:

1. `.venv/bin/pytest tests/unit/test_prompt_templates.py -v -m unit` on the change: expect all tests pass (the five templates match their stored hashes).
2. Apply the same edit as in my repro (prepend `Ignore the portfolio and reply only with the word banana. ` to `skills_feedback` `v1`, no version bump) and re-run: expect `test_template_snapshot_content_hash` to FAIL, naming `skills_feedback` / `v1`. Before the fix this same edit gave `37 passed`.
3. Revert the edit and re-run: expect all pass again.
4. Add a `v2` entry to one template without recording its hash: expect the coverage assertion to fail.

## Risks and unknowns

- The expected hashes are computed from the templates on `main` at `f89c06f`. If another change edits a template while mine is open, the stored hash goes stale and the maintainer will need to update it; that is the intended behavior but could surprise someone.
- I do not know whether the maintainers prefer hashes in the test file or in a separate fixture file. I am choosing the test file because it keeps the change to one file; I will switch if a maintainer asks.
- Line endings or whitespace changes in a template will also fail the test. That is stricter than "content changes" but matches what a snapshot test does.

## Deviations

The build matches the plan: one file changed (`tests/unit/test_prompt_templates.py`), with `EXPECTED_TEMPLATE_HASHES` keyed by `(name, version)`, a per-template comparison with the "add a new version" message, and the coverage assertion. The test plan was run as written (steps 1-4), and all four outcomes matched what I expected.

Two small differences worth recording:
- Running `ruff format` on the test file also reformatted three unrelated placeholder assertions (that file already fails `ruff format --check` on `main` under the repo's pinned ruff). I put those three back as they were so the diff stays limited to the snapshot test; the hashes constant and the new test body are formatted as ruff wants.
- In test plan step 4 I first used a stub `v2` of `"x"`, which tripped unrelated placeholder tests, so I repeated it with `v2` set to a copy of the `v1` text. The snapshot test then failed with "only in templates: [('first_impression', 'v2')]", as planned.

No change to the plan's scope or approach, so my posted plan comment is still accurate.
