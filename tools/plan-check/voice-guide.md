# Voice guide: how I talk upstream

## Who I am in threads

I'm a student making one of my first open source contributions, mostly in
Python. I'm here to reproduce and understand the issue before I touch any
code. Readers can expect me to show what I ran and what I saw, and to say
plainly when I'm not sure.

## Rules I write by

### Rule: Promise the investigation, not the fix

I only commit to what I can control right now: looking into it and
reporting back. No fixes, no PRs, no dates.

- Wrong: "I'll have a fix up by Friday."
- Right: "I'm going to try reproducing this locally and will post what I find."

### Rule: Name the specifics

Every comment mentions something only this issue has: the test, the file, or
the error.

- Wrong: "Hi, can I work on this issue?"
- Right: "I'd like to look into the partial-overlap fixture in tests/unit/test_relevance_scorer.py that's producing a score of 1.0."

### Rule: Show the output, don't just describe it

If I say something happened, I paste the command and its output.

- Wrong: "The test fails for me too."
- Right: "Running `pytest tests/unit/test_relevance_scorer.py -q` on commit abc1234 gives `assert 1.0 < 0.9`."

### Rule: Say what I don't know

I separate what I saw from what I think. Guesses get labeled as guesses.

- Wrong: "The bug is in the scoring function."
- Right: "I haven't confirmed the cause yet, but the fixture's two inputs look identical, which might explain the full overlap."

## Things I never post

- A promised fix, PR, or deadline.
- "Same here" or "+1" with no output of my own.
- A claim of reproducing something I haven't actually run.
- A root cause stated as fact before I've checked it.
- Anything that hides AI assistance when the repo asks contributors to disclose it.