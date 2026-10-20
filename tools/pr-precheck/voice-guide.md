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

### Rule: The title says what changed (added for PRs)

A PR title tells a reviewer what the diff does, so they can check it in
thirty seconds. It names the thing changed, not my feelings about it.

- Wrong: "Fixed the bug!!"
- Right: "test(rag): make the partial-overlap fixture genuinely partial"

### Rule: The description promises exactly the diff (added for PRs)

Every change the description names is in the diff, and every file in the
diff is named in the description. I don't say "nothing else changed" unless
I've checked `git diff main...HEAD`.

- Wrong: "Fixes the scorer and cleans up the tests."
- Right: "Changes one file, `tests/unit/test_relevance_scorer.py`: the fixture's query, and the removed xfail marker."

### Rule: State a shortfall as a fact, not an apology (added for PRs)

If I didn't run something, or I ran it differently than planned, I say what
and why in one plain sentence, and point to where it's recorded.

- Wrong: "Sorry, I wasn't able to test everything, hopefully it's fine."
- Right: "I ran `pytest tests/unit -m unit` instead of `make test-unit`, because `make` isn't set up on my Windows machine; it's the same command the Makefile runs."

## Things I never post

- A promised fix, PR, or deadline in a comment before the work exists.
- "Same here" or "+1" with no output of my own.
- A claim of reproducing something I haven't actually run.
- A root cause stated as fact before I've checked it.
- Anything that hides AI assistance when the repo asks contributors to disclose it.
- A PR description that claims coverage the test evidence doesn't show
  ("all tests pass" when I only ran one file).
- A checked box on the PR template for a check I didn't run.
