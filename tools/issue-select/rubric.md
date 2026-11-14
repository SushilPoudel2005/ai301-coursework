# Rubric: is this a good first issue?

All recency thresholds are measured against the capture date (eval mode) or today (live mode).

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Issue open | Issue header line: "state open/closed" | State is open. | required |
| Maintainer commits | "last 5 default-branch commits" in repo facts | The newest default-branch commit is at most 90 days before the capture date AND at least one of the 5 commits is authored by a human (username not ending in `[bot]`). | required |
| Maintainer responsive | "maintainer first-response sample" in repo facts, plus author_association (OWNER/MEMBER/COLLABORATOR) on comments in this thread | Ignore sampled issues opened fewer than 7 days before the capture date (too young for "no reply" to mean anything). Of the rest, at least 1 got a maintainer reply within 45 days, OR a maintainer commented in this thread, OR the issue itself was opened by an OWNER/MEMBER/COLLABORATOR and carries a `good first issue` label AND the newest default-branch commit is at most 60 days old (a maintainer who wrote and labeled the issue recently is engaged even if no one has replied yet). If no sampled issue is old enough to count, pass only when the commit check shows 3+ commits by humans in the last 5. A sample where every countable issue is "no maintainer comment" fails. | required |
| Repo in use | repo line (archived flag), "latest release", "last push to any branch" | Not archived AND (latest release within 365 days OR last push within 90 days). Archived always fails. | required |
| Bounded scope | Issue body and comment thread | One bounded piece of work a newcomer can finish in a single PR. Fails if: it is an umbrella/tracking issue (list of sub-items to be split up); the thread shows the design is still being debated with no maintainer decision; a maintainer says the fix needs core-internals changes; or it is a usage/support question rather than a change request; or it has been open more than 1 year AND its linked PRs include 2 or more closed-unmerged attempts (abandoned attempts signal real difficulty); or it was opened by a bot account (username ending `[bot]`) with no maintainer comment or maintainer-applied label, or its key input is still "TBD" (an unvetted request). A terse body, a missing repro, or a maintainer-opened bug that lists several possible causes or suggestions for one symptom is NOT a fail by itself. | required |
| Unclaimed | "this issue: assignees / linked PRs" in repo facts, and claim comments in the thread ("I'll take this", "working on this", "can I work on this", a PR mentioned in comments) | No assignee, no open linked PR, and no claim comment from someone else within 30 days before the capture date that is not followed by an abandonment/release of the claim or a maintainer reassigning it. When the sidebar and thread disagree, believe the thread. A closed unmerged PR alone does not fail this check. | required |
| AI contribution policy | "contribution policy" line in repo facts | The policy does not outright ban AI-generated or AI-assisted contributions. Conditions (disclosure, review, understand and test your changes) pass; silence passes. | required |
| Good-first-issue signal | Labels on the issue; whether the opener is a maintainer | Carries a good first issue / help wanted type label, or was opened by a maintainer with clear acceptance criteria. | preferred |
| Clear acceptance | Issue body | States what done looks like (expected behavior, checklist, or example). | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. `unclear` on a required check counts as fail. Preferred checks never change the verdict; they only rank accepted issues.
