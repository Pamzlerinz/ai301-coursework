# Plan: issue #64, partial-overlap fixture has full overlap

Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64
Branch: `fix/64-partial-overlap-fixture` (on my fork, from `main` at `99673c7`)

## Cause

The scorer is right and the test fixture is wrong. `RelevanceScorer.score`
in `rag/evaluator/relevance_scorer.py` returns the share of query words that
appear in the chunk (`overlap / len(query_tokens)`). The fixture in
`test_query_with_partial_overlap` scores the query "Python Django web
framework" against "Django is a Python web framework for rapid development",
which contains all four query words, so 4/4 = 1.0 is the correct answer and
`assert 0.3 < score < 0.9` fails.

This is grounded in my reproduction (posted on the issue): running with
`--runxfail` shows `assert 1.0 < 0.9` and the log line
`avg_score=1.0 chunks_count=1 query_len=4`. The `query_len=4` and the 1.0
together mean every query word matched.

## Change

One bounded change, in `tests/unit/test_relevance_scorer.py` only:

1. Change the fixture's query to "Python Django REST API". Against the same
   chunk, only "python" and "django" match, so the expected score is
   2/4 = 0.5, inside the test's 0.3 to 0.9 range.
2. Remove the `@pytest.mark.xfail(strict=True, ...)` marker from
   `test_query_with_partial_overlap`. The marker is strict, so once the
   fixture is fixed the test would pass and strict xfail would report that
   as a failure (XPASS(strict)).

Not in scope: any change to `RelevanceScorer` or its tokenizer (for example
punctuation handling or stemming), and any other test in the file. The
issue says the scorer behaves correctly, and my repro agrees.

## Test plan

Re-run my Unit 2 repro commands on the branch:

- `pytest tests/unit/test_relevance_scorer.py -q -rx` should report
  `19 passed` with no xfailed test.
- `pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap`
  should report `1 passed`, and the captured log should show
  `avg_score=0.5`.

Then run `make test-unit` to check nothing else in the unit suite changed.

## Unknowns

- I am on Windows; I have not checked that `make test-unit` runs here, so I
  may run the equivalent `pytest -m unit` instead and will say so in the PR.
- I have not checked whether the course repo's CI or a manifest tracks the
  xfail marker for this test. If removing it breaks a check, I will report
  that here before changing anything else.

## Deviations

Nothing changed in the fix; the plan held. The branch changes only
`tests/unit/test_relevance_scorer.py`: the new query and the removed xfail
marker, and the results matched the test plan (`19 passed`,
`avg_score=0.5`).

Two small things about how I ran the checks, neither of which changes the
plan:

- I ran `pytest tests/unit -v -m unit` (what `make test-unit` runs) instead
  of `make`, as the Unknowns section said I might. Before the change it gave
  `375 passed, 53 xfailed`; after, `376 passed, 52 xfailed`, so the only
  test that moved was this one.
- I added `-rP` to the `-k partial_overlap` command, because pytest only
  prints a passing test's captured log with that flag, and the log line is
  where `avg_score=0.5` shows.

The unknown about CI tracking the xfail marker resolved itself:
`pyproject.toml` says "Fixing a seeded bug should remove ... the test's
xfail marker", and nothing else in the repo references this test's marker.
