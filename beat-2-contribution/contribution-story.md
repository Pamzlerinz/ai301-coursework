# Contribution: Allow Image Widgets to be sorted by translation percentage instead of alphabetically

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** [Student name]
**GitHub Username:** Pamzlerinz
**Issue:** https://github.com/WeblateOrg/weblate/issues/11161
**Pull Request:** https://github.com/WeblateOrg/weblate/pull/22856

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 9: [one line on the tool work]**

[2-4 sentences: what you built (the scout, your strategy), what broke
or fell short, what changed because of it.]

**Unit 10: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 11: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 12: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

[1-2 paragraphs in your own words: how you searched, what your scan
surfaced, what your scout's verdict said, why this issue fits your
profile, and what you hope to learn. If your scout's verdict on it
was reject and you chose it anyway, say so here and say why; choosing
against your own tool silently is what loses points. Close with an
honest read of the tool itself: where the scout fell short on a real
wild search and what you changed or would change, or what convinced
you it held up. **Record the link to your claim comment in this section too.**
Not claimed yet? Write `claim pending` and today's date where the link goes,
and add the link the moment your claim lands.]

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- OS: Ubuntu 26.04.1 LTS under WSL2 (kernel 6.18.33.2-microsoft-standard-WSL2) on Windows 11
- Versions: Python 3.14.4, Django 6.0.10, PostgreSQL 18.6, Valkey 9.0.4, uv 0.13.8
- Project: Weblate `main` at `ecc00b2e9f0aa701869ebf8d915705e686a06614` (2026-11-15), cloned from my fork Pamzlerinz/weblate
- Setup, following `docs/admin/install/venv-debian.rst`, `docs/contributing/start.rst`, and `docs/contributing/tests.rst`:
  1. `apt install` the build libraries from `venv-debian.rst`, plus `postgresql`, `valkey-server`, `gettext`, and `libicu-dev`
  2. `uv sync --all-extras --dev`
  3. Create a PostgreSQL superuser `weblate` with password `weblate` (what `scripts/test-database.sh` expects), then `. scripts/test-database.sh`
  4. `uv run ./manage.py collectstatic --noinput`
  5. Baseline: `uv run pytest weblate/trans/tests/test_widgets.py` gives `293 passed, 187 subtests passed`

Challenges and fixes:
- Weblate does not run on native Windows (it needs PostgreSQL, Valkey, and Linux-only libraries). GitHub Codespaces refused with `HTTP 402: ... out of monthly free usage`, so I installed Ubuntu in WSL (`wsl --install -d Ubuntu`).
- The first `uv sync` failed building `pyicu`: `Please install pkg-config on your system or set the ICU_VERSION environment variable`. Installing `libicu-dev`, which is not in the docs' package list, fixed it.

**Steps to Reproduce**

1. Set up the environment above and run `. scripts/test-database.sh`.
2. Add a local test, `weblate/trans/tests/test_repro_11161.py`, built on `FixtureTestCase`. It stores a different `translated` count on each non-English translation's stats (highest for the alphabetically first language), calls `self.project.stats.update_stats()`, requests `widget-image` for the project with `widget="multi"`, `color="auto"`, `extension="svg"`, and prints each language name and percent in the order the SVG draws them.
3. Run `uv run pytest weblate/trans/tests/test_repro_11161.py -q -s`.
4. Observed: the languages come out alphabetically (Czech 75% above English 100%), not by percentage, and no URL parameter changes the order.

**Reproduction Evidence**

- **Commit showing reproduction:** none (the reproduction test is local only and excluded from git)
- **Screenshots/logs:**

  ```
  $ uv run pytest weblate/trans/tests/test_repro_11161.py -q -s
  rendered order (language, percent):
    Czech        75%
    English      100%
    German       25%
    Italian      25%
  alphabetical?            True
  sorted by percent desc?  False
  ```
- **My findings:** Reproduced on current `main`. The order comes from `MultiLanguageWidget.render()` in `weblate/trans/widgets.py`, which iterates over `sort_unicode(self.get_language_stats(), lambda x: str(x.language))`; `MatrixMultiLanguageWidget.render()` has the same line. One thing that didn't match what I set up: I gave German a higher count than Italian, but both rendered as 25%. Faking stats with `stats.store()` doesn't reliably reach the project-level numbers, which I confirmed later when the same approach made Czech drop out of my first regression test (see Implementation Notes). I posted the output on the issue: https://github.com/WeblateOrg/weblate/issues/11161#issuecomment-6906037699

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

The language bar chart widgets always list languages alphabetically. In `weblate/trans/widgets.py`, `MultiLanguageWidget.render()` (the `multi` "Vertical language bar chart" widget, which `HorizontalMultiLanguageWidget` inherits) iterates over `sort_unicode(self.get_language_stats(), lambda x: str(x.language))`. `MatrixMultiLanguageWidget.render()` uses the same expression. Nothing reads a request parameter for ordering, so there is no way to get a "top languages" chart.

The maintainer asked on the earlier attempt (PR #20392) for the option to be exposed in the UI, like PR #20257 (capitalize badges). Widgets declare UI options in `extra_parameters`; `weblate/trans/views/widgets.py` `widgets()` already sends them to the widgets page, and `weblate/static/js/widgets.js` already renders `boolean` parameters as a checkbox and adds them to the embed URL (added by #20257).

**Proposed Solution**

Add a boolean `sort_by_percent` entry to `MultiLanguageWidget.extra_parameters`, so the vertical, horizontal, and matrix widgets all offer a "Sort by translation percentage" checkbox. Add `MultiLanguageWidget.get_sorted_language_stats(request)`, which keeps the alphabetical sort and, when `?sort_by_percent=1` is set, re-sorts by `translated_percent`, highest first. `sorted()` is stable, so ties stay alphabetical. I asked on the issue whether that tie-break is what the maintainers want and have no answer yet. Both `render()` methods call the helper instead of their inline sort.

Not changing: the default order (still alphabetical without the parameter), other widgets, `widgets.js` and `views/widgets.py` (boolean parameters already work end to end), and how empty translations are skipped.

**Implementation Plan**

1. `weblate/trans/widgets.py`: add the `sort_by_percent` extra parameter and `get_sorted_language_stats()` to `MultiLanguageWidget`; use it in `MultiLanguageWidget.render()` and `MatrixMultiLanguageWidget.render()`.
2. `weblate/trans/tests/test_widgets.py`: add `WidgetsSortByPercentTest` with fixed stats (Czech 25%, German 50%, Italian 50%, Japanese 100%) covering the default order, the sorted order with the tie, all three widgets, and the rendered SVG.
3. `docs/changes.rst`: one line under "New features" in the unreleased 2026.12 section.

Deviation (2026-11-16): my first test faked stats with `translation.stats.store()` on the fixture project. That was unreliable: Czech came out at 0% and disappeared from the widget in both orders. The test now patches `get_language_stats()` with fixed stats instead. Second deviation: mypy flagged `"Widget" has no attribute "get_sorted_language_stats"` in the new test, because `WIDGETS[name]` is typed as the base class, so I added an `isinstance` narrowing in a second commit (`9cd7f2c`).

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

**1. The reproduction, before and after.** Before, on `main`: the widget draws Czech 75% above English 100% (output in Reproduction Evidence). The new regression tests, run against `main`'s `widgets.py`, fail, and the rendered SVG with `?sort_by_percent=1` ignores the parameter:

```
$ uv run pytest weblate/trans/tests/test_widgets.py -k SortByPercent -q
E       AssertionError: Lists differ: ['Czech', 'German', 'Italian', 'Japanese'] != ['Japanese', 'German', 'Italian', 'Czech']
7 failed, 2 passed, 293 deselected in 10.20s
```

After, on `feat/11161-widget-sort-by-percent`:

```
$ uv run pytest weblate/trans/tests/test_widgets.py -k SortByPercent -v
...::WidgetsSortByPercentTest::test_default_order_is_alphabetical PASSED
...::WidgetsSortByPercentTest::test_sort_by_percent PASSED
...::WidgetsSortByPercentTest::test_sort_by_percent_rendered PASSED
3 passed, 293 deselected, 6 subtests passed in 7.75s
```

With the parameter the SVG lists Japanese (100%), German (50%), Italian (50%), Czech (25%): highest first, with the German/Italian tie alphabetical. The control, `test_default_order_is_alphabetical`, shows the order without the parameter is still Czech, German, Italian, Japanese for all three widgets.

**2. The repo's own checks** (my ship manifest, from Weblate's AGENTS.md):

- `uv run pytest weblate/trans/tests/test_widgets.py`: `296 passed, 193 subtests passed` (293 before, plus 3 new)
- `uv run prek run --from-ref main --to-ref HEAD`: every hook passed (ruff check, ruff format, reuse lint, Sphinx Lint, codespell, typos)
- `uv run mypy` on the two changed files: no errors in either. Full mypy already fails on `main` with 248 errors in 35 untouched files, so I scoped it.
- Not run locally: the full test suite (it would take far longer than my setup allows); CI runs it on the PR.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

**2026-11-15**

Ran my first wild scan with issue-scout and graded about 25 Issue Selection site picks. Most already had open PRs or weren't Python-first.

**2026-11-16**

Claimed power-grid-model-io#107, then found PR #540 had been opened 27 minutes before my claim. Posted a withdrawal and switched to weblate#11161. Before claiming, a GraphQL query found the stale PR #20392 and nijel's request to expose the option in the UI, so I planned to follow PR #20257's pattern. Posted my claim.

**2026-11-16**

Forked and cloned Weblate. Native Windows can't run it and Codespaces was out of free hours, so I set it up in WSL Ubuntu (`libicu-dev` was needed for `pyicu`). Baseline: 293 widget tests pass. Reproduced the alphabetical-only order with a local test and posted the output on the issue.

**2026-11-16**

Built the fix on `feat/11161-widget-sort-by-percent`. Found that boolean widget parameters already work end to end since #20257, so no JavaScript or view changes were needed. My first regression test failed in a confusing way (Czech missing from both orders); the cause was the stats-faking setup, not the fix, so I rewrote the test to patch `get_language_stats()` with fixed stats. Confirmed the new tests fail on `main` and pass on the branch. mypy flagged the test's widget type, so I narrowed it in a second commit.

**2026-11-16**

Wrote my ship manifest (widget tests, prek, scoped mypy, each run in WSL against the exact commit). plan-check accepted the plan. The ship-gate's first run stopped on my own pr-precheck: `description-honest` couldn't verify the sentence "The commits carry an `Assisted-by: Claude Code` line" from the commit list, so I removed it. The second run passed. Opened PR #22856.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** https://github.com/WeblateOrg/weblate/pull/22856

**PR Description:** Title: "feat(widgets): allow sorting language bar charts by translation percentage". Opens with `Fixes #11161`. Sections: What this changes (a `sort_by_percent` boolean in `MultiLanguageWidget.extra_parameters` and `get_sorted_language_stats()`, used by the `multi`, `horizontal`, and `matrix` widgets; ties stay alphabetical; default order unchanged; follows nijel's #20392 request via #20257's boolean parameters), Before and after (Czech 75% above English 100% on `main`; the new tests fail on `main` with `['Czech', 'German', 'Italian', 'Japanese'] != ['Japanese', 'German', 'Italian', 'Czech']` and pass on the branch), Checks (`test_widgets.py` 296 passed, prek hooks passed, mypy clean on the two changed files, full suite left to CI), and AI assistance.

**Maintainer Feedback:**

- 2026-11-16: none yet

**Status:** Awaiting review

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

- Weblate's `AGENTS.md` (commit format, changelog rule, test and lint commands): https://github.com/WeblateOrg/weblate/blob/main/AGENTS.md
- Weblate's contributor docs for local setup and tests: https://docs.weblate.org/en/latest/contributing/start.html and https://docs.weblate.org/en/latest/contributing/tests.html
- nijel's review on the earlier stale PR, asking for the option in the UI: https://github.com/WeblateOrg/weblate/pull/20392
- PR #20257 (capitalize badges), the pattern I followed for a boolean widget parameter: https://github.com/WeblateOrg/weblate/pull/20257
