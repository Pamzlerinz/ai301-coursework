# Procedure: how this skill grades a plan package

## Read order

1. Read the repo-facts block first. Write down the contribution policy's
   requirements for comments word for word (AI disclosure, human-written
   comments, templates), or "none stated".
2. Read the issue title and body. Write down, in one line, the behavior the
   issue reports.
3. Read the thread highlights. For each comment by an OWNER, MEMBER,
   COLLABORATOR, or maintainer, write down what it says to do or what it
   rules (diagnosis, requested approach, request for testing, "working as
   intended"). Write down any PR number mentioned. Also note any cause a
   non-maintainer claims, labeled as a claim.
4. Read the repro-evidence block. For each step, write down what it shows,
   and mark every control or comparison (a run with one factor changed) with
   what it rules in or rules out. Write down the "Actual" and "Expected"
   lines.
5. Only now read the candidate plan, then the candidate plan comment.

The order matters: the evidence and the thread are read before the plan, so
the plan's confident wording cannot set the frame for what the evidence
shows. cause-grounded and thread-engaged are both graded against notes made
before the plan was seen.

## Evidence gathering

For each check, pull these facts from the package (in live mode, from the
locations in `references/evidence-guide.md`):

- cause-grounded: quote the plan's stated cause. Next to it, list the
  controls from read-order step 4 and whether each is consistent with that
  cause.
- scope-bounded: list every change the plan commits to, one per line. Mark
  each as "fix", "test", "doc for the changed behavior", "deferred", or
  "extra" (anything else the issue did not ask for).
- executable: quote the files, functions, or code sites the plan names, and
  the approach it commits to. Quote any open choice or "somewhere".
- test-decisive: quote the test plan. Write down the observable outcome it
  names, or "none".
- thread-engaged: from read-order step 3, list the maintainer directions and
  PRs. For each, quote where the plan or comment mentions it, or write "not
  mentioned".
- conventions-met: from read-order step 1, list each requirement, and quote
  the part of the plan comment that meets it, or write "missing".
- unknowns-stated and no-overpromise: quote any coverage claim or timeline in
  the plan or comment.

## Check execution

1. Run the checks in the rubric's table order, one at a time, using only the
   facts gathered for that check. Re-read a part of the package only if the
   gathered note for that check is empty.
2. Apply the check's pass condition exactly as written in `rubric.md`. If
   the gathered facts meet it, grade `pass`; if they meet a fail condition,
   grade `fail`.
3. If the evidence for a check is genuinely absent from the package (for
   example the plan names no cause, or no test), grade `fail`, not
   `unclear`: absence is a fact. Grade `unclear` only when the evidence is
   present but the rubric's condition cannot be decided from it.
4. For every grade, write one line of evidence: the quote or fact that
   decided it. For a fail, name the specific change, step, control, or
   requirement that caused it.
5. Do not let one check's result change another's. A plan with a correct
   cause can still fail scope-bounded; a plan with a perfect comment can
   still fail test-decisive.

## Verdict assembly

1. Convert `unclear` grades using the rubric's unclear rule: unclear on
   cause-grounded, scope-bounded, executable, or test-decisive becomes fail;
   unclear on thread-engaged becomes pass only if the thread has no
   maintainer comment and no PR, and unclear on conventions-met becomes pass
   only if no policy is stated. Otherwise unclear becomes fail.
2. If any required check is fail after step 1, the verdict is `reject`.
   Otherwise it is `accept`. Preferred checks are reported but never change
   the verdict.
3. In the summary before the JSON, name the deciding check (the first
   required check that failed, or "all required checks passed") and quote
   its evidence line.
4. Emit the JSON block from SKILL.md last, with every check listed in table
   order, including preferred ones, using the grades from before step 1's
   conversion.
