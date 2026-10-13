# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: in an eval bundle, the candidate plan's "Cause" or
  "Diagnosis" section (or the reason given for its change), read against the
  repro-evidence block's steps, timings, control runs, and "Actual" line, and
  against any cause the thread highlights propose. In live mode, the draft
  plan.md's cause statement, read against the student's posted repro comment
  on the issue and the issue thread.
- What good looks like: the cause explains every result the repro reports,
  including its controls. A control is the strongest evidence in the package:
  if the repro shows the same input working when one factor is removed, or
  the fault already present at an earlier step, the cause must sit where the
  control points. A plan that blames a component the control shows working,
  or that adopts a confident thread diagnosis the repro's own measurements
  contradict, is not grounded however polished it is.

## Scope

- Where it lives: in an eval bundle, the candidate plan's change list and its
  "In scope" / "Not in scope" (or "Out") lines, read against the issue title
  and body and the repro's "Actual" line. In live mode, the same sections of
  the draft plan.md, read against the issue.
- What good looks like: one bounded change that fixes the reproduced
  behavior, plus its test and any doc update for the changed behavior.
  Larger work is named and deferred with a reason. A drive-by rewrite looks
  like a correct fix bundled with refactors, upgrades, migrations, new
  options or settings, UI changes, or CI work the issue never asked for.

## Executability

- Where it lives: in both modes, the plan's "Change" or "Approach" steps and
  any file paths, functions, or modules it names.
- What good looks like: a named location (file, function, or clearly
  identified code site) and one chosen approach, in an order a stranger
  could start on today. Vague looks like "somewhere in the editor code",
  open either/or choices, or an investigation presented as a plan.

## Test plan

- Where it lives: in both modes, the plan's "Test" or "Test plan" section,
  read against the repro evidence's steps and its "Expected" line.
- What good looks like: the test re-runs the repro (or a named regression
  test does the same) and says what the reader will observe when the fix
  works: an exit code, a value, an output line, a color or state change at a
  named step. A vague test plan names only "the full suite passes" or a
  feeling such as "should feel fast".

## Honesty

- Where it lives: in both modes, the plan's risks, unknowns, and deferral
  notes, and in live mode the "Deviations" section of plan.md after the
  build.
- What good looks like: what was not tested (a platform, a variant, a side
  effect) is said plainly as unknown or deferred. False confidence looks like
  coverage claims beyond what the repro ran. A deviation found mid-build is
  written into plan.md's Deviations section with what changed and why.

## Comms

- Where it lives: in an eval bundle, the candidate plan comment read against
  the thread highlights (comments labeled OWNER, MEMBER, or COLLABORATOR,
  and any PR numbers mentioned) and against the repo-facts block's
  "contribution policy" line. In live mode, the draft plan comment read
  against the live issue thread and the repo's CONTRIBUTING.md and AI policy
  files.
- What good looks like: the comment shows it read the thread. If a
  maintainer gave a diagnosis, a direction, a ruling, or asked for testing,
  or a PR already exists, the comment names it and follows it or explains
  the divergence. Every requirement the contribution policy states is met;
  when the policy requires AI disclosure, the comment discloses it (every
  eval package counts as AI-assisted). Boilerplate that ignores the thread
  is not thread-aware.
