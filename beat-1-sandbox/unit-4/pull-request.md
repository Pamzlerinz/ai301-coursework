# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-howard/pull/83

**Branch**

fix/64-partial-overlap-fixture

**pr-precheck verdict on my draft**

I ran my own `pr-precheck` in live mode on the draft twice before opening.

1. First run: `accept`, with all seven required checks passing. The voice check flagged two things. The draft claimed mypy passed "as the Makefile runs it" but only showed output for CI's form of the command, which breaks my rule "Show the output, don't just describe it". And it called the change "the two-line change" when the diff is +1/−5. I also noticed that the checks block showed `pytest tests/unit -q` while the checklist said `-m unit`. I fixed all three: I added the Makefile-form mypy output and both pytest commands with their results, and I described the change exactly.
2. Second run, on the revised draft: `accept`, all required checks pass, no voice rules broken. The deciding line was "all required checks passed". Its house-rule notes: a classmate's PR (#80) is open on the same issue, which the scope's house rule says does not block mine, and the CI box has to turn green once the workflows run on the opened PR.

---

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

0. Smoke run on the four calibration packages, `--only calib-01,calib-02,calib-03,calib-04 --include-calibration` (unscored): all four agreed with the worksheet labels (calib-01 accept; calib-02, calib-03, calib-04 reject), `agreement: 0/0 scored items`. Before the full run, I loosened one check (repro-rerun; see Check rationale) based on how it read calib-03.
1. Full run, saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the run in `eval-run.txt`, and the rubric, procedure, evidence guide, and SKILL.md in `tools/pr-precheck/` are unchanged since (their sha256 fingerprints match the run's header).

**Package analysis**

pkg-20 (ghostty-org/ghostty#13604, mode 2031 reports the wrong color scheme when only one non-conditional theme is configured). Category: standards-wall. Gold label: reject. My rubric: reject. They agree.

The fix itself is good. My tool passed plan-fidelity ("Only Config.zig and its test block change; the key != .theme hunk is the planned fix and the tests are for it"), repro-rerun (the after-result `ESC[?997;1n` matches the plan's expected `?997;1n`, and the conditional-pair control is stated as unchanged), repo-checks-run ("zig build test passes (config suite included)."), and diff-clean. A rubric that only read the diff and the evidence would have accepted it.

The deciding check was template-and-policy. The repo-facts block says: "contribution policy (CONTRIBUTING.md + AI_POLICY.md): strict AI rules. All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". The description and the commit message say nothing about AI. My rubric counts every package as AI-assisted work, and its pass condition fails "if AI use goes undisclosed where the policy asks for disclosure", so the grader wrote "AI_POLICY requires disclosure of the tool and extent of assistance; the description and commit contain none." That is what the gold label is built around: the standards wall is an ask the repo states out loud, and a correct fix does not get past it.

My tool also failed description-honest on pkg-20, and I think that read is stricter than I meant. The description says "a second case covers the single-theme scheme flip", and the grader pointed out that the new second test checks `.fullscreen = true` (unrelated keys), not the scheme flip. That's true: the scheme-flip case is the rewritten first test, and the description mislabels which test is new. But it's a mislabel of something that is in the diff, not a claim about a change that isn't there. The verdict doesn't depend on it, because template-and-policy alone holds the package.

**Check rationale**

Check `repro-rerun` (required), pass condition as it reads in `tools/pr-precheck/rubric.md`:

> Passes if the plan's main repro is shown re-run on the branch with an observable after-result (an output line, an exit code, a value, a visible state) that matches the expected-after the plan states, AND every other failure mode, variant, or control the test plan names is either shown with its after-result or stated plainly as not run, with a reason. An after-result that says it matches a specific expected output the plan or issue writes out ("matches the issue's expected output byte for byte") is observable. For a control (a case that already worked and must keep working), naming it and saying it is unchanged is enough. A new test in the diff that asserts the fixed behavior counts for a failure mode only if its passing outcome is shown in the evidence. Fails if the evidence is missing or says "(none provided)", if the after-result is a feeling or a bare "works" / "fixed" / "looks right" with nothing a reader could check, if the after-result shown does not match what the plan expected, or if a failure mode the test plan names is silently missing from the evidence.

Why it reads this way: the check is built around the plan's test plan, not around "is there test output". calib-04 is the reason. Its plan names two failure modes "by name" (the channel-capacity overflow and the thread-spawn abort), and its evidence re-runs only the first, then says "`cargo test` passes (614 tests)". A check that asked "does the PR show a before/after?" would pass that. So the check lists every failure mode the test plan names and needs an after-result for each, which is the "silently missing from the evidence" clause. The "stated plainly as not run, with a reason" clause is the honest-outcome rule: a PR that says "I couldn't run repro 2 because ..." is disclosing, not hiding.

The two sentences in the middle were added after the calibration smoke run. In my first version, the grader failed calib-03's repro-rerun because its after-result was "matches the issue's expected output byte for byte" and its control was "All-comments control unchanged", and it said neither gave "an output line, exit code or value". calib-03 is a reject anyway (an unplanned rewrite of `property.js` under a description that says "with no changes beyond it"), but that read was wrong on its own terms: the issue writes out the expected output, so "matches it byte for byte" is something a reader can check. And requiring full output for a control that already worked would push clear-accept PRs into rejects. I rejected the other fix I considered, dropping the "observable" requirement and accepting any stated after-result, because that would pass "should work" and "the library update looks right" (calib-02).

**Trade-offs**

The calib-03 revision loosened repro-rerun, and it was made before my only full run, so I didn't use an `--only` canary. The full run was the check: not-tested held at `not-tested 4/4`, and calib-04 still fails on the missing second failure mode, which the loosening doesn't touch. The case I accept missing: a PR that falsely claims "matches the expected output byte for byte", or says a control is "unchanged" without having run it, now passes repro-rerun on that claim alone. My tool can't catch a confident false claim about evidence it never sees; the eval bundle has no way to re-run the command. The other cost is on the strict side. description-honest failed pkg-20 for mislabelling which test was new, which means a clear-accept PR with a small wording mix-up between two real tests could be held. It didn't happen on this set (`clear-accept 7/7`), but it's the first place I'd look if a later run rejected a good PR.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
