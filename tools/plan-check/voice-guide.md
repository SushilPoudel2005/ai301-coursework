# Voice guide: how I talk upstream

## Who I am in threads

I am a student making my first open-source contribution, working through a course. I say that plainly, I say what I have actually done, and readers can expect a short comment with evidence and a specific next step.

## Rules I write by

### Rule: Only claim what I ran

Say what I did and saw, not what I assume. If I have not reproduced it yet, I promise to, I do not assert it.

- Wrong: "I can confirm this bug happens on the latest version."
- Right: "I'm going to reproduce this on the current main branch and will post my environment and output here."

### Rule: Name the specific behavior

Every comment mentions this issue's concrete symptom so it could not be pasted on another issue.

- Wrong: "I'd like to work on this issue."
- Right: "I'd like to take this: the second review for the same user returns the first review's cached result."

### Rule: State the next step and when

End a claim with one concrete next action and a time frame I can keep.

- Wrong: "I'll look into it soon."
- Right: "I'll post a repro report with my environment and output within three days."

### Rule: Own words, disclose AI help

Comments are my own account. If the repo's policy asks, I say which AI tool helped and how.

- Wrong: a comment pasted straight from an assistant with no mention.
- Right: "I used an AI assistant to help draft the test; I ran it and understand each line."

## Things I never post

- "+1", "same here", or "same as above, can confirm" in place of my own proof.
- A promise with no date, or one I cannot keep.
- Confident root-cause claims I have not traced.
- Words in the maintainers' voice that I did not write or read myself.
