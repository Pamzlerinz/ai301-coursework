---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: is this pull request ready to
submit? A PR package is a candidate pull request (its title, description,
commit list, diff, and test evidence), read against the plan it claims to
implement (with any deviation notes) and the issue that plan belongs to
(with its thread and the repo's stated PR template and contribution policy).

Do not answer any other question. Do not grade whether the issue was worth
taking, whether the plan was a good plan, or whether the code is how you
would have written it; the plan is the yardstick here, not the thing being
graded. Grade one package per run. If you are handed more than one PR, or
something that is not a PR package, say so and stop.

## Inputs and modes

Decide the mode first. If you were handed an eval package bundle (a
markdown file headed "Eval package: <id>"), you are in eval mode.
Otherwise you are in live mode.

**Live mode** grades the student's own PR before it is opened. Gather these
five inputs, and name each one's source in your summary:

1. The plan: the student's `plan.md`, including its Deviations section. In
   the course repo it sits at `beat-1-sandbox/unit-3/plan.md`; if the
   student names another path, use that one.
2. The diff: everything the branch changes relative to the repo's default
   branch. Produce it by running `git diff main...HEAD` (three dots) from
   the student's working copy of the Path Review repo, with the fix branch
   checked out. Also run `git log main..HEAD` for the commit list.
3. The draft PR title and description: the draft file the student points
   you to, or the text they paste. If there is no draft, stop and ask for
   one; do not grade a PR description you have not seen.
4. The test evidence: the Testing section of the draft description, plus
   any captured terminal output the student points you to.
5. The issue: the issue the plan belongs to, read on the scoped repo (via
   `gh issue view <n> --comments`, or the web if `gh` is not available),
   with its thread. Also read the repo's own asks:
   `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the
   working copy.

A house-chain student (one the instructor routed to the house issue) has no
`plan.md` of their own: read the house plan and the house repro pack in its
place. The same checks grade the same things there.

**Eval mode** grades a frozen package bundle. The bundle is the whole world:
every fact comes from the bundle text. Fetch nothing, run nothing, and read
no other file. Always grade the complete package: every check in the rubric
and the full verdict rule, even when an early check already decides the
verdict.

## The scope seam (live mode only)

In live mode, read `scope.md` in this tool's directory before anything
else. Then:

- If its `Repo:` line still carries a bracketed placeholder (such as
  `<ORG>/<PATH-REVIEW-REPO>`), stop without grading. Tell the student to
  fill the `Repo:` line in `scope.md` with their section's Path Review
  repo. Never guess a scope.
- If the PR would target any repo other than the one on the `Repo:` line
  (check the working copy's `origin` remote and the issue URL), refuse to
  grade and say which repo is in scope.
- Apply the house rules scope.md lists when reading evidence: the PR comes
  from a `fix/<issue-number>-<slug>` branch on the student's fork, one PR
  per issue, the PR template is always used, and a classmate's PR on the
  same issue does not block this one (so it does not fail thread-engaged).
  Report any house rule the package breaks in the summary.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md` in this tool's directory: the
student's own rules for how they write upstream. Hold the draft PR title and
description against each rule. In the summary, under a "Voice" heading, list
every rule the draft breaks: quote the rule's name and the sentence in the
draft that breaks it. If none are broken, say so in one line.

The voice guide never changes the verdict on its own: it is personal, and no
check in `rubric.md` reads it. A draft can break a voice rule and still be
ready to submit. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

The tool is four files, and you execute them; you do not replace them with
your own judgment.

- `rubric.md` defines the checks (each with its evidence, pass condition,
  and weight) and the verdict rule, including how `unclear` is treated. It
  decides what passes.
- `references/evidence-guide.md` maps where each evidence family lives in an
  eval bundle and in live mode, and what good looks like there. Use it to
  find the evidence each check names.
- `procedure.md` is the operating procedure: the read order, the gathering
  move for each check, how a check executes, and how grades become the
  verdict. Execute it step by step, as written.

Where the procedure is silent on a step you need, do not invent one. Take
the most literal reading of the rubric, and in the summary write
"Procedure gap:" followed by what was missing.

If `rubric.md` has no checks, or no verdict rule, or `procedure.md` has no
steps, refuse to grade: say which file is empty and that this tool cannot
grade without it. Do not make up checks or steps to fill the hole.

## Verdict and output

The verdict is binary. `accept` means the PR is ready to submit. `reject`
means hold it. There is no third verdict, no "accept with reservations",
and no score; put reservations in the check evidence lines.

Before the JSON, write a short readable summary: one line per check (name,
grade, the deciding fact), the deciding check named per `procedure.md`, and
in live mode the voice notes and any house-rule notes. Then end the reply
with this fenced JSON block, valid, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Use the bundle id as `item` in eval mode, and the PR's URL (or, before it
is opened, the branch name) in live mode. List the checks in rubric table
order, with the exact check names from `rubric.md`.

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote that
  decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. A terse, complete PR can be ready, and a
  long, confident one can be hiding drift. Read the diff against the plan
  and the evidence against the test plan; never reward formatting, length,
  or confidence.
- The rubric decides, not you. If a check passes by its stated condition but
  feels wrong, it still passes. Note the tension in the summary if you want;
  the fix belongs in `rubric.md`, not in this run.
- The procedure decides how, not you. Follow `procedure.md` as written, and
  report its gaps instead of quietly working around them.
- Honest shortfalls are not failures. A deviation, a skipped step, or a
  deferred edge that the PR or the plan states openly is honest work, and is
  graded by the rubric's conditions, not held against the PR by default.
- Unclear defaults to fail. Treat `unclear` as the rubric's verdict rule
  directs. Where the rule is silent, an unverifiable claim is a failing one:
  a PR you cannot verify from the package is not ready to submit.
