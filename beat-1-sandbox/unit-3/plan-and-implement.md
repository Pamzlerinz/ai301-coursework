# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Pamzlerinz

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

---

## Your branch

**Branch**

fix/64-partial-overlap-fixture

**Evidence**

Before: `main` at `99673c7f53aa3c4666f99d38641a6ef0512a22e6`, unchanged (PowerShell, Windows 11, Python 3.14.5 venv).

```
> git rev-parse HEAD
99673c7f53aa3c4666f99d38641a6ef0512a22e6
> .venv\Scripts\python -m pytest tests/unit/test_relevance_scorer.py -q -rx
..x................                                                      [100%]
=========================== short test summary info ===========================
XFAIL tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - issue #64: relevance scorer 'partial overlap' fixture actually has full overlap
18 passed, 1 xfailed in 2.09s
> .venv\Scripts\python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap
F                                                                        [100%]
================================== FAILURES ===================================
_____________ TestRelevanceScorer.test_query_with_partial_overlap _____________

self = <tests.unit.test_relevance_scorer.TestRelevanceScorer object at 0x0000023884A52D70>
scorer = <rag.evaluator.relevance_scorer.RelevanceScorer object at 0x0000023884A7B8C0>

    @pytest.mark.xfail(
        strict=True,
        reason="issue #64: relevance scorer 'partial overlap' fixture actually has full overlap",
    )
    def test_query_with_partial_overlap(self, scorer):
        """Test query with partial overlap returns score between 0 and 1."""
        query = "Python Django web framework"
        chunks = [
            {"text": "Django is a Python web framework for rapid development"},
        ]
    
        score = scorer.score(query, chunks)
    
        assert isinstance(score, float)
        assert 0.0 <= score <= 1.0
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
        ^^^^^^^^^^^^^^^^^^^^^^^^
E       assert 1.0 < 0.9

tests\unit\test_relevance_scorer.py:58: AssertionError
---------------------------- Captured stdout call -----------------------------
2026-10-12 20:24:50 [info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
=========================== short test summary info ===========================
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap
1 failed, 18 deselected in 0.96s
```

After: branch `fix/64-partial-overlap-fixture`, commit `be895b0` (query changed to "Python Django REST API", strict xfail marker removed). Same commands; `-rP` added to the second so pytest prints the passing test's log line.

```
> git branch --show-current
fix/64-partial-overlap-fixture
> .venv\Scripts\python -m pytest tests/unit/test_relevance_scorer.py -q -rx
...................                                                      [100%]
19 passed in 1.73s
> .venv\Scripts\python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap -rP
.                                                                        [100%]
=================================== PASSES ====================================
_____________ TestRelevanceScorer.test_query_with_partial_overlap _____________
---------------------------- Captured stdout call -----------------------------
2026-10-12 20:28:44 [info     ] relevance_scored               avg_score=0.5 chunks_count=1 query_len=4
1 passed, 18 deselected in 0.59s
```

Unit suite (`pytest tests/unit -q -m unit`, what `make test-unit` runs), before then after the change:

```
375 passed, 53 xfailed, 1 warning in 27.59s
376 passed, 52 xfailed, 1 warning in 14.80s
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

0. Smoke test, `--only calib-01,calib-02,calib-03,calib-04 --include-calibration` (unscored): all four agreed with the worksheet labels, `agreement: 0/0 scored items`.
1. Full run, first rubric: `agreement: 18/19 scored items`. pkg-02 errored (`claude exited 1: Warning: no stdin data received in 3s`), so the harness would not save the run. The one disagreement was pkg-14 (gold accept, `failed: cause-grounded`). Categories: `clear-accept 5/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
2. `--only pkg-02,pkg-14,pkg-07,pkg-16,calib-03 --include-calibration` after loosening cause-grounded: `agreement: 2/3 scored items`. pkg-02 errored again; pkg-14 now `failed: executable`; canaries pkg-07, pkg-16, and calib-03 still rejected.
3. `--only pkg-02,pkg-14,pkg-10,pkg-17,pkg-18` after loosening executable, run with `PYTHONUTF8=1` (pkg-02 contains an emoji that Windows' default encoding could not pass to `claude`): `agreement: 5/5 scored items`.
4. Full run saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, colour strings leaking into the pane on SSH reattach). Gold label: accept. My rubric rejected it in run 1 and run 2, and accepted it in run 4.

The plan's cause is that the reattach path "wires the client's stdin to the session before the query responses have been consumed". The repro has two controls: 0.44.1 is clean, and "After `rm -rf ~/.cache/zellij` (thread observation): the next attach is clean, the one after leaks again." The plan addresses the second one directly: "The cache control fits: with an empty cache the color data is refetched along the fresh-attach path once." In run 1 the grader failed cause-grounded with "the cache-clear control ... points at the cache; the plan covers it only with an unevidenced ... claim". My check said every control had to be "consistent with the cause", and the grader read "consistent" as "proven". But nothing in the repro contradicts the plan's explanation: the cache clear only buys one clean attach, which is what a fresh-attach path would do, and the 0.44.1 control fits a regression in the 0.44.2 reattach path. After I said that an explained, uncontradicted control counts, run 2 passed cause-grounded but failed executable, because the plan says "exact functions to be pinned in the PR after tracing". It does name the code site, "the client attach/reattach path in `zellij-server` (session connection handling)", and it commits to one approach (drain OSC responses before wiring pane input). So it is buildable; a stranger knows where to start. Once executable said that naming the module and code path is enough when the approach is fixed, the rubric accepted it, matching gold.

**Check rationale**

Check `cause-grounded` (required), pass condition as it now reads in `tools/plan-check/rubric.md`:

> Passes if the stated cause explains what the repro evidence shows and nothing in the repro evidence rules it out: every control run, comparison, or variant the repro reports (for example "same input without flag X works", "with colors off it is instant", "the value is already wrong at step 4") is consistent with the cause the plan names. A cause taken from the thread or a maintainer passes only if the repro evidence is also consistent with it. A control the plan explicitly addresses with an explanation that nothing in the repro contradicts counts as consistent, even if that explanation is not yet proven; the plan does not have to prove its cause, only not be ruled out by its own evidence. Fails if any repro step or control contradicts the cause (the plan blames a component the evidence shows behaving correctly, or a stage the evidence shows the fault already happened before), if the plan ignores the repro's evidence and adopts a cause it does not support, or if the plan names no cause at all. Confidence, polish, and length do not count toward passing.

Why it reads this way: the check is built around controls because that is what the wrong-cause category tests. In calib-03 the plan adopts the thread's confident key-binding diagnosis, but the repro's step 3 shows the same 26-second cost "with no pager in the loop at all". A check that only asked "does the plan cite evidence?" would pass that plan, because it does cite the thread. So the check makes the controls the deciding evidence, and the sentence "A cause taken from the thread or a maintainer passes only if the repro evidence is also consistent with it" is there for exactly that trap. The sentence about explained controls was added after run 1, when pkg-14 was rejected for not proving its explanation of the cache control. I rejected the other fix I considered, requiring the plan to show evidence for its cause, because at the planning stage nobody has built the fix yet; a plan can only be held to not contradicting its own evidence.

**Trade-offs**

Both revisions loosened a check, so I re-ran canaries before the confirming full run. For cause-grounded, I re-ran pkg-07 and pkg-16 (wrong-cause, already agreeing) and calib-03, the worksheet's wrong-cause trap: all three still rejected in run 2. For executable, I re-ran all three unbuildable packages, pkg-10, pkg-17, and pkg-18: all still rejected in run 3. The final full run kept `wrong-cause 4/4` and `unbuildable 3/3`. The case I accept missing: a plan that explains away a control with a story that sounds right but is wrong would now pass cause-grounded, as long as nothing in the repro directly contradicts the story. And a plan that names only a module, not a function, now passes executable, which could let through a plan whose "module" is too large to really start from.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
