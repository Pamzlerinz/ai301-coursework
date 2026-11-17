# Contribution: Allow Image Widgets to be sorted by translation percentage instead of alphabetically

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Kafilat Sarki-Umar
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

**Unit 9: Built issue-scout and ran my first wild scan**
What I built: issue-scout is my tool. It searches for candidate issues using my search strategy, then grades them with my rubric. 
What it produced: My first wild scan (2026-11-15) graded its top 5 results. Three were rejected because they already had open PRs, and it accepted a junk issue labeled `invalid` and `duplicate`. After that I graded about 25 picks from the Issue Selection site one at a time; most were out because they already had open PRs or weren’t Python-first.
What broke or fell short: I claimed power-grid-model-io#107, then found PR #540 had been opened 27 minutes before my claim. The scout’s “unclaimed” read was out of date by the time I acted on it, and I posted a withdrawal.
What changed because of it: In the tool, I added `-linked:pr -label:invalid -label:duplicate -label:wontfix` to both queries, made the styling filter match whole words, and replaced `scope-bounded` with `scope-feasible` plus a difficulty note. In my process, before claiming weblate#11161, I ran a GraphQL query for prior PRs. It found the stale PR #20392 and nijel’s request for a UI option. That check is still manual; it isn’t part of the scout yet.

**Unit 10: voice-guard stops two drafts, memory file fixed**
voice-guard: I merged my three copies of the voice guide into one file and added 4 “Upstream” rules for comments I post on real projects. I installed it as a hook, so it checks a draft automatically before I post.
The memory file (CLAUDE.local.md): notes about the Weblate repo that Claude reads at the start of each session, such as setup steps and commands.

What broke, and what I changed
The install failed on Windows. It expected python3 and symlinks, and Windows has neither. I changed it to python and used pointer files instead.
The guard stopped two real drafts:
21:29: My claim comment listed what my PR would contain, which breaks my rule "promise the investigation, not the fix."
21:58: My update showed output without the command that produced it.
I fixed both before posting.
It wasn’t consistent. It passed the same claim text a minute before it stopped it.
The memory file had a mistake. It said Weblate needed cairo, which is wrong for the current main. I corrected it and added the WSL commands.



**Unit 11: ship-gate and my manifest**
What I built
The manifest: the list of checks that must pass before a PR goes out. It has 3 checks, all run in WSL through weblate-check.sh:
the widget tests;
prek on the changed files;
mypy on the changed files.
3 things I left out on purpose, each with a reason:
the full test suite, which is too slow on my setup;
prek --all-files, which rewrites files I didn’t touch;
full mypy, which already fails on main with 248 errors in 35 files.
The ship-gate: it runs the manifest, then my own pr-precheck, and stops the PR if anything fails.

What happened

The install broke the same way again. It wrote python3 into the hook, and I changed it to python, just like in Unit 10.
Run 1: all 3 checks passed, then it stopped on my own pr-precheck. description-honest couldn’t verify “The commits carry an Assisted-by line” from the commit list. I removed the sentence.
Run 2: PASS, 8/8. thread-engaged came back unclear, which my rubric counts as a pass. Then I opened PR #22856.
An honest note: I created the PR using gh’s full path, so the hook didn’t run on the create step itself.

---

## Why I Chose This Issue
how I found it and why I chose it
How I searched: one issue-scout scan, plus about 25 picks from the Issue Selection site graded directly, with a Python filter.
What I found: most of the picks already had open PRs or weren’t really Python projects.
#107: the scout accepted it and I claimed it, but PR #540 had been opened 27 minutes earlier, so I withdrew.
The scout’s verdict on #11161: ACCEPT, with all 5 required checks passing and difficulty “moderate”.
My main 3 reasons for picking this issue: AI allowed, available, Python.
I wanted to learn how much of this project I could help out on and how searching for issues can be difficult.

What it missed:
“Unclaimed” is only a snapshot; #107 was taken minutes later.
It couldn’t see PR #20392, which was linked only in the sidebar. Only GraphQL showed it.
What I changed:
added a “scope-feasible” check and a difficulty note;
added -linked:pr to the searches;
made it a habit to re-check, including GraphQL, right before claiming.

https://github.com/WeblateOrg/weblate/issues/11161#issuecomment-6905645265

**Toolkit**

- Tool: issue-scout. I ran one scan with the `gh search issues` queries in my search-strategy.md, then graded about 25 Issue Selection site picks one at a time in direct mode, reading each thread and its linked PRs.
- What it produced: ACCEPT on #11161, with all five required checks passing and a "moderate" difficulty note. It had also accepted power-grid-model-io#107, which I lost to PR #540.
- Difficulties: `unclaimed` is a point-in-time check, so it couldn't see PR #540 being opened 27 minutes before my claim on #107. It also missed PR #20392 on #11161, which was linked through the Development sidebar and only showed up through a GraphQL query. The site's Python filter returned many repos whose main language isn't Python.
- What changed: I replaced `scope-bounded` with `scope-feasible` plus a difficulty note, added `-linked:pr` and junk-label exclusions to my queries, and started re-checking linked PRs (including through GraphQL) right before claiming. That last step is still manual.

---

## Understanding the Issue

**Problem Description**
what’s broken or missing: Weblate’s language bar chart widgets (the images projects embed to show translation progress) can only list languages alphabetically.
There’s no way to order them by how translated they are, so I can’t make a “top languages” chart.


**Expected Behavior**
what should happen: An option to sort languages by translation percentage, highest first.
The option shows up in the UI as a checkbox on the widgets page. nijel asked for this on PR #20392.
Without the option, the default order stays alphabetical.


**Current Behavior**
what actually happens: Languages are always A–Z. In my repro, Czech at 75% is drawn above English at 100%.
No URL parameter changes the order.


**Affected Components**
which parts of the codebase are involved: weblate/trans/widgets.py: the render() methods of MultiLanguageWidget and MatrixMultiLanguageWidget, which hold the sort line. The horizontal widget inherits from MultiLanguageWidget.
weblate/trans/views/widgets.py and weblate/static/js/widgets.js, which show the options on the widgets page.
weblate/trans/tests/test_widgets.py, for the tests.
docs/changes.rst, for the changelog line.


**Toolkit**

- Tool: Claude Code with my CLAUDE.local.md for Weblate. I had it read only the widget code (`widgets.py`, `views/widgets.py`, `widgets.js`, `test_widgets.py`) and the maintainer's example PR #20257, then wrote what it found into the memory file.
- What it produced: The file map in Affected Components, the exact sort line in `MultiLanguageWidget.render()`, and #20257 as the pattern to copy.
- Difficulties: The memory file's first version said the widgets need cairo/PyGObject. That came from older code and was wrong for current `main`, and it briefly sent my setup in the wrong direction.
- What changed: I corrected CLAUDE.local.md, and I now check imports on the exact commit I'm working on before writing them down as facts.

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

- Tool: repro-check, run on my claim draft before posting (claim-only mode), then on the full package (claim + repro report). The voice guard checked each comment before it went up.
- What it produced: ACCEPT on the claim alone, then ACCEPT on the full package with all six required checks passing.
- Difficulties: repro-check passed my first Weblate claim, but the voice guard stopped the same text for listing what a PR would contain, which breaks my rule "promise the investigation, not the fix." The two tools disagreed. The guard also stopped my update for showing output without the command that produced it.
- What changed: I reworded the claim to say what a change looks like it would touch, added the pytest command above the output in my update, and put the full test code in the report so someone else can rebuild it.

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

- Tool: plan-check, run on `plan.md` and a draft `plan-comment.md`, following the read order in my `procedure.md` (evidence and thread before the plan).
- What it produced: ACCEPT with all six required checks passing. The preferred check `unknowns-stated` failed because my plan described the tie-break (equal percentages stay alphabetical) as settled, when the maintainer hadn't answered my question yet.
- Difficulties: I wrote the plan before nijel answered, so the plan's confident wording got ahead of what I knew.
- What changed: I added the tie-break to `plan.md` as an open question and said the same in the PR description.

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

- Tool: My new regression tests plus the three checks in my ship manifest (widget tests, prek on changed files, mypy on the two changed files), all run in WSL Ubuntu.
- What it produced: The new tests fail on `main` (7 failed, 2 passed) and pass on the branch (3 passed, 6 subtests). The full widget suite gives 296 passed, prek passes, and mypy shows no errors in the changed files.
- Difficulties: My first test faked stats with `translation.stats.store()`. Czech came out at 0% and vanished from the widget in both orders, which looked like a bug in my fix but was a bug in the test setup.
- What changed: I rewrote the test to patch `get_language_stats()` with fixed stats, then confirmed the tests fail without the fix and pass with it.

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

- Tool: Claude Code, driving WSL Ubuntu through small `.sh` script files, on branch `feat/11161-widget-sort-by-percent`.
- What it produced: Two commits: `8740190` (the feature, tests, and changelog line) and `9cd7f2c` (a mypy type narrowing in the tests).
- Difficulties: PowerShell kept breaking inline `wsl ... bash -c` commands with quoting errors, and mypy flagged `"Widget" has no attribute "get_sorted_language_stats"` in my new test.
- What changed: I moved every WSL command into a script file and added an `isinstance` check in the test so mypy knows the widget type.

---

## Pull Request

**PR Link:** https://github.com/WeblateOrg/weblate/pull/22856

**PR Description:** Title: "feat(widgets): allow sorting language bar charts by translation percentage". Opens with `Fixes #11161`. Sections: What this changes (a `sort_by_percent` boolean in `MultiLanguageWidget.extra_parameters` and `get_sorted_language_stats()`, used by the `multi`, `horizontal`, and `matrix` widgets; ties stay alphabetical; default order unchanged; follows nijel's #20392 request via #20257's boolean parameters), Before and after (Czech 75% above English 100% on `main`; the new tests fail on `main` with `['Czech', 'German', 'Italian', 'Japanese'] != ['Japanese', 'German', 'Italian', 'Czech']` and pass on the branch), Checks (`test_widgets.py` 296 passed, prek hooks passed, mypy clean on the two changed files, full suite left to CI), and AI assistance.

**Maintainer Feedback:**

- 2026-11-16: The cubic-dev-ai review bot flagged that my fake stats were already in alphabetical order, so the default-order test couldn't catch a missing sort; nijel applied its suggested reorder as commit `104dfec`. nijel then commented on `widgets.py`: "I think using the string `sort` parameter would be a better approach, as it will allow us to extend in the future. The boolean flag is bounded to just this configuration."
- 2026-11-16: Not answered yet.

**Status:** Awaiting review

**Toolkit**

- Tool: The ship-gate (my manifest's checks, then my pr-precheck on the branch, plan, evidence, and description), run by hand twice before opening the PR.
- What it produced: Run 1: all three repo checks passed, then STOP, because my pr-precheck marked `description-honest` unclear on "The commits carry an `Assisted-by: Claude Code` line", which it couldn't verify from the commit list. Run 2: PASS. Then PR #22856 opened.
- Difficulties: The hooks didn't fire on the actual `gh pr create`, because `gh` was called by its full path. Both had passed by hand on the same commit, title, and description just before.
- What changed: I removed the sentence pr-precheck couldn't verify from the description.

---

## Learnings and Reflections

**Technical Skills Gained**
1. Setting up a big Linux-only project on Windows with WSL: PostgreSQL, Valkey, uv.
2. Reading a large codebase by one subsystem (the widgets) instead of the whole thing.
3. Writing Django tests, and mocking with patch.object to give a test fixed stats.
4. mypy type narrowing with isinstance.

**Challenges Overcome**
1. Losing #107. PR #540 went up 27 minutes before my claim. I withdrew, switched to Weblate, and started checking for PRs right before claiming.
2. Weblate on Windows. Native Windows couldn’t run it, and Codespaces was out of free hours. I moved to WSL, and pyicu needed libicu-dev, which isn’t in the docs.


**What I'd Do Differently Next Time**
1. Start earlier so I can work on an issue that hasn't been worked on yet
2. Check for linked PRs (including GraphQL) right before claiming.
3. Set up the Linux environment in case the issue needs it, even though I am not picking an issue using Linux


---

## Resources Used

- Weblate's `AGENTS.md` (commit format, changelog rule, test and lint commands): https://github.com/WeblateOrg/weblate/blob/main/AGENTS.md
- Weblate's contributor docs for local setup and tests: https://docs.weblate.org/en/latest/contributing/start.html and https://docs.weblate.org/en/latest/contributing/tests.html
- nijel's review on the earlier stale PR, asking for the option in the UI: https://github.com/WeblateOrg/weblate/pull/20392
- PR #20257 (capitalize badges), the pattern I followed for a boolean widget parameter: https://github.com/WeblateOrg/weblate/pull/20257
