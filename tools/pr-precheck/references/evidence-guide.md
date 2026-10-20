# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives: in an eval bundle, the "Plan context" block (the plan's
  change list, its "Files:" line, its "Scope" and "Not in scope" lines, and
  any deviation note), read against the candidate PR's "Diff" section (each
  `--- a/` / `+++ b/` file header and each `@@` hunk) and its "Description".
  In live mode, the student's `plan.md` (its Change, Not in scope, and
  Deviations sections), read against `git diff main...HEAD` run from the
  branch's working copy, and the draft PR description.
- What good looks like: every file header in the diff is a file the plan
  names, a test file for the fix, a doc or changelog entry, or a file a
  deviation note names with a reason; every step in the plan's change list
  shows up as a hunk, or the PR says it was dropped and why. Silent drift in
  the "more" direction looks like a correct fix plus a hunk in a file the
  plan never mentioned (a refactor, a dependency bump, a reformatted
  neighbor). In the "less" direction, it looks like a plan step (a test, an
  entry, a second failure mode) with no hunk and no note. A description
  that says "exactly the plan" or "no other changes" over a diff with an
  extra hunk is silent drift, not a wording problem.

## Test evidence (harness category: not-tested)

- Where it lives: in an eval bundle, the candidate PR's "Test evidence"
  section, read against the plan's "Test plan" sentence and the "Repro
  evidence" paragraph at the top of the plan context (its commands, its
  failure modes, its controls, and its expected-after). The repo's own
  checks are named in the repo-facts block ("pull requests" and
  "contribution policy" lines) and in the plan's test plan ("go test",
  "cargo test", "the format suite"). In live mode, the draft description's
  Testing section and the student's captured terminal output, read against
  `plan.md`'s Test plan and the student's posted repro; the repo's checks
  are in `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and
  `.github/workflows/`.
- What good looks like: the repro command from the plan, re-run on the
  branch, with a before-result and an after-result a reader could check (an
  output line, an exit code, a value), and the after-result is the one the
  plan expected. When the test plan names more than one failure mode,
  variant, or control, each one has its own after-result, or a plain line
  saying it was not run and why. The repo's suite or checks are named with
  their outcome (a pass count, "passes", a CI status). Not tested looks like
  "(none provided)", "should work", "the library update looks right", a
  claim about one failure mode when the plan named two, or only the suite
  passing with no repro re-run.

## Diff quality (harness category: unreviewable)

- Where it lives: in both modes, the diff's hunks (every `+` and `-` line)
  and the commit list (in live mode, `git log main..HEAD` and
  `git diff main...HEAD`).
- What good looks like: every changed line is part of the fix, its test, or
  its doc entry, so a reviewer reading the diff sees only the change. The
  debris tells are: added `print`/`console.log`/`dbg!`/debug logging,
  commented-out code, new TODO/FIXME hacks, editor or OS files, build
  output, lockfile churn unrelated to the fix, and formatting-only or
  reshuffled lines (statements merged onto one line, reordered
  assignments, a function rewritten in a new style) in code the fix does
  not need. Commit messages like "gofmt" or "cleanup" next to the fix are a
  hint to look for that churn.

## Standards and comms (harness category: standards-wall)

- Where it lives: in an eval bundle, the repo-facts block: the "pull
  requests" line (template sections, checklist items, warnings such as
  "may be closed without review") and the "contribution policy" line
  (contribution guide asks, AI-use policy), plus the "Thread highlights"
  (comments marked OWNER, MEMBER, or COLLABORATOR, and any PR numbers).
  Read them against the PR's title, description, and commits. In live mode,
  the repo's `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`
  (branch naming, commit convention, the green-CI and xfail rules), any AI
  policy file, and the live issue thread, read against the draft title and
  description.
- What good looks like: each section and checklist item the template names
  appears in the description with real content or an honest "n/a" and a
  reason; required entries (changelog, issue reference) are present; when
  the policy asks for AI-use disclosure, the description says how AI was
  used; any maintainer direction in the thread is followed or named. A
  visibly ignored ask looks like a description with no template sections
  when the template warns that such PRs are closed, a checklist left as the
  template's placeholder, or a policy's disclosure line with no disclosure.
  (Whether the description's claims match the diff is plan fidelity, above.)
