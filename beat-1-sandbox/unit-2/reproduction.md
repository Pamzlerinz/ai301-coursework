# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Pamzlerinz

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6008580001

Hi! I'd like to look into this as my first contribution. The fixture in `test_query_with_partial_overlap` (tests/unit/test_relevance_scorer.py) scores the query "Python Django web framework" against a chunk that contains all four of those terms, so a 1.0 looks like the scorer behaving correctly rather than a scorer bug. I'm going to reproduce it locally first and will post what I find here.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6008591514

Reproduction report: I was able to reproduce this. One difference from the issue: on the current commit, the test is now marked `xfail`, so the issue's exact command no longer shows the failure without an extra flag. Details below.

**Environment**
- OS: Windows 11 (10.0.26200)
- Python 3.14.5 (in a fresh venv)
- Commit `99673c7f53aa3c4666f99d38641a6ef0512a22e6` (default branch, cloned 2026-10-05)

**Steps** (from a fresh clone, in PowerShell)
```
git clone https://github.com/codepath/pathreview-ai301-fa26-howard
cd pathreview-ai301-fa26-howard
python -m venv .venv
.venv\Scripts\python -m pip install -e ".[dev]"
.venv\Scripts\python -m pip install pytest
.venv\Scripts\python -m pytest tests/unit/test_relevance_scorer.py -q -rx
.venv\Scripts\python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap
```

**What I saw**

The issue's command reports the test as an expected failure rather than a failure:
```
XFAIL tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - issue #64: relevance scorer 'partial overlap' fixture actually has full overlap
18 passed, 1 xfailed in 1.11s
```

Running it with `--runxfail` shows the failure the issue describes:
```
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests\unit\test_relevance_scorer.py:58: AssertionError
------------------------------------ Captured stdout call ------------------------------------
[info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - assert 1.0 < 0.9
```

**Result:** reproduced. The scorer returns 1.0 for the query "Python Django web framework" against the chunk "Django is a Python web framework for rapid development", which contains all four query terms, so the assertion `0.3 < score < 0.9` fails. This matches the issue's description that the fixture, not the scorer, is the problem.

I haven't changed anything yet. One thing I noticed: because the marker is `strict=True`, I think the `xfail` decorator would need to come off along with a fixture fix, or the test would start reporting as an unexpected pass. I'll confirm that before going further.

## Eval iterations

**Run history**

1. Full run with my first rubric (saved): `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. Disagreements: pkg-03 (`failed: claims-supported`), pkg-09 (`failed: behavior-matches, claims-supported`), pkg-12 (`failed: steps-followable`), all gold accept. Categories: `clear-accept 5/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
2. `--only pkg-03,pkg-09,pkg-12` after loosening steps-followable, behavior-matches, and claims-supported: `agreement: 1/3 scored items`. pkg-03 now agreed; pkg-09 still `failed: claims-supported`, pkg-12 still `failed: steps-followable`.
3. `--only pkg-03,pkg-09,pkg-12` after a second revision of steps-followable and claims-supported: `agreement: 2/3 scored items`. pkg-12 now agreed; pkg-09 still `failed: claims-supported`.
4. Full run saved with `--save-run eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. Only miss: pkg-05 (`failed: steps-followable`).

**Package analysis**

pkg-09 (sharkdp/fd#2033, `--exec-batch` ordering). Gold label: accept. My rubric rejected it in runs 1 to 3 and accepted it in run 4.

The package is an honest cannot-reproduce. The report opens with "Result: I could NOT reproduce scenario 2", shows the exact commands and the `order.log` output, says "I did not test scenario 1", and has a section "What differed from the report's conditions" admitting its padding approach "may not achieve" the trigger. In run 1 my behavior-matches check failed it because it required a cannot-reproduce to show the run "at that exact trigger", and the report itself says it may not have hit the exact trigger. So honesty about uncertainty counted against it. My claims-supported check failed it because it required "every claim" to have an artifact. After I loosened both, claims-supported still failed it in runs 2 and 3, because the report says "I ran this 5 times" but shows one output, and says fd "appears to flush both command buffers at the same file-count boundary", which the grader read as an unsupported root-cause claim. That reading is wrong: one output representing repeated runs is normal, and "appears" marks a hedged guess, not a claim of fact. Once the check said both of those explicitly, run 4 accepted it, matching gold.

**Check rationale**

Check `claims-supported` (required), pass condition as it now reads in `tools/repro-check/rubric.md`:

> Passes if the report's main outcome is backed by an artifact in the report: "reproduced" only when the artifact shows the issue's behavior; "could not reproduce" only when the artifact shows the attempt and the report does not overstate it (it says what it did not test or what may have differed). One representative artifact can stand for repeated runs the report summarizes in prose. Supporting observations in prose, and hypotheses clearly hedged as guesses ("appears", "may", "I think"), are fine as long as nothing in the artifacts contradicts them. Fails if the report claims more than its artifacts show: "reproduced" over output of a different behavior, a root cause stated as fact without evidence, results claimed for versions or platforms the report says were not tested, or a cannot-reproduce presented as proof the bug does not exist.

Why it reads this way: my first version passed only if "every claim is backed by an artifact in the report". That rejected pkg-03, a correct ripgrep reproduction, over one side note in prose: "Dropping `-r '$1'` from the same command reports 1, 4, 7, 10 correctly". So the second version narrowed the requirement to the report's main outcome and allowed supporting prose observations. That fixed pkg-03 but not pkg-09, so the third version added that one artifact can stand for repeated runs, and that hedged hypotheses are not claims of fact. I rejected making this check preferred or removing it, because it is the honesty check: the fail list still catches a confident reproduction of the wrong behavior and an unhedged root cause, which is what the wrong-target category tests.

**Trade-offs**

The looser claims-supported trusts prose more. A report could say "I ran this 5 times" without having done it, or bury a weak root-cause guess behind "appears", and this check would pass it. I accept that miss, because demanding an artifact for every sentence rejected good reports. To check that loosening did not open the door to bad packages, my final full run kept every reject-side category intact: `no-evidence 4/4` and `wrong-target 4/4`, plus `disclosure 1/1` and `unfollowable-comms 3/3`. The cost showed up elsewhere: pkg-05, which agreed in run 1, flipped to reject in run 4 on steps-followable, the other check I rewrote. I did not re-run a canary on it before the full run, so I cannot yet tell whether my new steps wording ("Fails only if the reader must invent something") reads pkg-05 as missing a step, or whether it is run-to-run variance in the grader.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.