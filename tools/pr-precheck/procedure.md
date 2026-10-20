# Procedure: how this tool grades a PR package

## Read order

1. Read the repo-facts block (live: the PR template, `docs/CONTRIBUTING.md`,
   and any AI policy file). Write down every stated ask word for word: each
   template section, each checklist item, each required entry (changelog,
   tests, issue reference), the AI-use policy, and the repo's own checks or
   suite. If there are none, write "none stated".
2. Read the issue and the thread highlights. Write down the behavior the
   issue reports in one line. For each OWNER, MEMBER, COLLABORATOR, or
   maintainer comment, write down what it asks for or rules. Write down any
   PR number mentioned. If there are none, write "no maintainer direction".
3. Read the plan context (live: `plan.md`). Write down, as separate lists:
   the plan's change steps; the files it names; its "not in scope" items;
   any deviation note; and, from its test plan and repro evidence, each
   failure mode, repro command, and control, with the expected-after result
   for each.
4. Read the diff. For each file header, write down the file and what its
   hunks do in a few words. Note any debug line, commented-out code, or
   formatting-only hunk as you see it.
5. Read the commit list.
6. Read the test evidence.
7. Read the title and description last.

The order matters: the plan is read before the diff so the diff is judged
against what was promised, not the other way round, and the description is
read after the diff and the evidence so its confident wording cannot set the
frame for what the diff contains. plan-fidelity, repro-rerun, and diff-clean
are graded from notes made before the description was read.

## Evidence gathering

For each check, build these notes from the package (live: from the places
named in `references/evidence-guide.md`):

- plan-fidelity: a two-column pairing. Left: every diff file and hunk from
  read-order step 4. Right: the plan step, test, entry, repo ask, or
  deviation note it maps to, or "unplanned". Then a second list: every plan
  step from step 3, with the hunk that delivers it, or "missing", and
  whether the PR mentions the gap.
- description-honest: list each factual claim in the title and description
  (what changed, what was added, "nothing else", "fixes X"). Next to each,
  write the diff hunk or evidence line that confirms it, or "not in diff" /
  "contradicted by <hunk>".
- repro-rerun: from step 3, list each failure mode, repro command, and
  control the test plan names. Next to each, quote the after-result from
  the test evidence, or write "not shown" or "stated as not run: <reason>".
  Note whether each after-result matches the expected-after.
- repo-checks-run: from steps 1 and 3, list the repo's suite and checks.
  Quote where the evidence or description names each one and its outcome,
  or write "not mentioned".
- diff-clean: the debris notes from step 4, each with its file and the line
  quoted.
- template-and-policy: from step 1, each ask, with the part of the
  description or commits that meets it quoted, or "missing", or "placeholder".
- thread-engaged: from step 2, each direction or PR, with where the PR
  follows it or names it quoted, or "not mentioned".
- title-specific: the title, quoted.

## Check execution

1. Run the checks in the rubric's table order, one at a time, using only the
   notes gathered for that check. Re-read a part of the package only when
   the note for that check is empty.
2. Apply the check's pass condition exactly as `rubric.md` writes it. If the
   notes meet the pass condition, grade `pass`; if they meet a fail
   condition, grade `fail`.
3. Before failing any check for a shortfall, look for a disclosure: a
   deviation note in the plan, or a sentence in the description or evidence
   saying what was not done and why. If one covers the shortfall, the
   shortfall does not fail the check.
4. If the evidence a check needs is absent from the package (no test
   evidence, no mention of the suite), grade `fail`, not `unclear`: absence
   is a fact. Grade `unclear` only when the evidence is present but the
   pass condition cannot be decided from it.
5. For every grade, write one line of evidence: the quote or fact that
   decided it. For a fail, name the hunk, claim, failure mode, or ask that
   caused it.
6. Grade each check on its own. A correct fix can still fail diff-clean; a
   tidy diff can still fail repro-rerun. One hunk can fail more than one
   check (an unplanned refactor fails plan-fidelity and diff-clean); grade
   each anyway.

## Verdict assembly

1. Convert `unclear` grades by the rubric's unclear rule: unclear on
   template-and-policy becomes pass only when the repo states no asks, and
   unclear on thread-engaged becomes pass only when the thread has no
   maintainer comment and no PR. Every other unclear on a required check
   becomes fail.
2. If any required check is fail after step 1, the verdict is `reject`.
   Otherwise it is `accept`. Preferred checks never change the verdict.
3. In the summary before the JSON, name the deciding check, which is the
   first required check in rubric table order that failed (or "all required
   checks passed" for an accept), and quote its evidence line.
4. Emit the JSON block from SKILL.md last, listing every check in rubric
   table order, preferred ones included, with the grades from before step
   1's conversion.
