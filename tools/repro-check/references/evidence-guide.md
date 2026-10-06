# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: in an eval bundle, the repro report's environment record
  (OS, runtime and language versions, package versions, commit or branch),
  read against the issue context and the repo-facts block for the target
  version or platform. In live mode, the draft repro comment, read against the
  issue body (versions or platforms the opener names) and the repo's README or
  CONTRIBUTING setup section.
- What good looks like: the OS, the runtime version, and the exact code
  version tested (commit hash, tag, or branch) are named, and they match the
  issue's target or the difference is stated. A stranger could rebuild the
  same setup from the record alone.

## Steps

- Where it lives: in an eval bundle, the repro report's steps or commands,
  plus any reproduction (input, script, commands) the issue itself gives. In
  live mode, the draft repro comment's steps, checked against the issue body
  and the repo's own setup docs.
- What good looks like: starting from a fresh setup, every action needed to
  reach the trigger is either written in the report, or covered by a
  reproduction the issue already provides that the report says it used
  verbatim, or by a repo doc the report points to. A reader never has to
  invent a step. Length and formatting do not matter.

## Behavior shown

- Where it lives: in an eval bundle, the artifacts in the repro report (output
  excerpts, logs, tracebacks, test results, screenshots), read against the
  issue's description of the bug. In live mode, the output pasted in the draft
  repro comment, read against the issue body.
- What good looks like: the artifact shows the same error message, failing
  assertion, wrong value, or symptom the issue describes, from the trigger the
  issue names. An adjacent behavior does not count: a different error, a
  different test, a crash during install or setup, or a warning near the bug.
  A cannot-reproduce shows a real attempt at the issue's trigger and what
  happened instead, and says honestly where its conditions may have differed.

## Honesty

- Where it lives: in both modes, the repro report's stated outcome
  ("reproduced", "could not reproduce", "partially") and any cause or scope
  claims, read against the artifacts in the same report.
- What good looks like: the main outcome points to an artifact that shows it.
  "Could not reproduce" with the attempt shown, and with what was not tested
  stated, is honest and complete. Side observations in prose are fine unless
  the artifacts contradict them. A report fails when its confidence outruns its
  evidence: "reproduced" over output that shows a different behavior, a root
  cause stated as fact with no evidence, or "affects all versions" when one
  was tested.

## Comms

- Where it lives: in an eval bundle, the claim comment read against the issue
  context, and both comments read against the repo-facts block's contribution
  policy (AI-use disclosure rules, templates, required fields). In live mode,
  the draft comments read against the issue thread and the repo's
  CONTRIBUTING.md, AI policy files, and issue or PR templates.
- What good looks like: the claim names this issue's specifics (its error,
  test, or file) and says what the commenter will do next, investigate or
  reproduce, without promising a fix or a date. Every requirement the repo's
  policy states is met; if it requires disclosing AI assistance, the comments
  disclose it. Boilerplate that could sit on any issue ("Can I work on
  this?") is not specific.