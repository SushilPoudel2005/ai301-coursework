# Evidence guide: where proof lives in a reproduction package

In an eval bundle the parts are: **Repo facts** (stated bug-report template asks, contribution policy), **Issue** (body, labels, version/OS the reporter states), **Thread highlights**, **Candidate claim comment**, **Candidate repro report**. Live, the issue side is the GitHub issue page, `CONTRIBUTING.md`, `.github/ISSUE_TEMPLATE/` and any AI policy file; the candidate side is the student's draft files, read the way a stranger on the thread will read the posted comments.

## Environment

- Where it lives: the first lines of the repro report (versions, OS, runtime). The target comes from the issue body (the version/OS the reporter names) and the repo-facts "bug reports" line (what the template asks for).
- What good looks like: exact versions (not "latest"), OS and runtime named, covering what the template asks; and they match the issue's target, or the report says what differs and why it still applies.

## Steps

- Where it lives: the commands or actions in the repro report, from setup to the trigger.
- What good looks like: written out in order, starting state stated, each command copyable. A stranger would not have to guess anything to get from a clean start to the behavior.

## Behavior shown

- Where it lives: the artifacts in the report (command output, logs, error text, a stated screenshot), read against the symptom in the issue body.
- What good looks like: the artifact contains the symptom the issue names (same error text, same wrong value, same missing element) and shows expected next to actual or a working control. Adjacent behavior is a fail: a different error, a nearby feature, a restatement of the issue in the report's own words with no output behind it.

## Honesty

- Where it lives: the sentences in the report and claim comment that state results, causes or scope, set beside the artifacts that are supposed to back them.
- What good looks like: every claim has output behind it. An honest "could not reproduce" with environment and output is a pass. Red flags: root causes asserted without a trace, "happens every time / for everyone / on all machines", confidence the artifacts do not show.

## Comms

- Where it lives: the claim comment against the issue; both comments against the repo-facts "contribution policy" and "bug reports" lines (and live, `CONTRIBUTING.md`, templates, any AI policy file).
- What good looks like: the claim names this issue's concrete behavior and the author's next step in their own words; it is not a +1 or boilerplate that fits any issue. AI policy: an outright ban on AI-generated comments fails anything presented as AI-written; a disclosure requirement that covers issue comments means the package must disclose tool and extent; own-words requirements mean the comments read as the author's account. A policy silent on comments, or no policy, passes.
