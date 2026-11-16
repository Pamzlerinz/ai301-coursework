# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Kafilat Sarki-Umar

## Featured Tools

The tools I chose to write on are issue-select, plan-check and pr-precheck. The tools that changed the most were plan-check and pr-precheck. plan-check was changed because of cause-grounded which needed every control to be "consistent with the cause", the grader read "consistent" as "proven" and rejected pkg-14. And pr-precheck was changed because repro-rerun loosened after the smoke run. The one which did the most contribution to my Weblate contribution was pr-precheck. The other tools are less interesting is because these tools helped me with the final issue i picked


**issue-select**

**What it is, and the judgment hole I wrote:** 
I worked on the issue-select as the first project on this list. It checks information about an issue, checks if the repo is archived, who opens an issue, the labels, comments, pull requests, assignees and the repo's AI rules. An example of what i decided was unclear, which was a rule "Unclear Handling", a check that asks “is this alive and available?” needs proof, so unclear means fail. A check that asks “is there a problem here?” needs a red flag to fail, so unclear means pass. Its judgment is based on my `rubric.md`, which has 8 checks, of which 5 are required. The 5 required are required because these are the important aspects that need to be checked, namely;
1. maintainer-alive: checks if a human has committed or commented recently.
2. repo-in-use: checks if the repo isn’t archived and has been pushed to recently.
3. scope-bounded: checks if the issue is one clear, small task. It isn’t a huge umbrella issue, an open design debate, an unapproved new feature, or something a bot opened.
4. unclaimed: checks if nobody is assigned and nobody is already working on it.
5. ai-policy-allows: checks if the repo doesn’t ban AI-assisted contributions.

**How it evolved, dated:** 
2026-09-29: issue-select was downloaded, then utilizing AI, ran the eval and it worked for 18/20. Issue-01 and Issue-20 were missed. issue-01 (conda#16475) should have been accepted, but my rubric rejected it. It’s a single docs task whose “Proposed changes” section lists a new page plus updates to existing pages. issue-20 (excalidraw#11811, “Add company logo shape to the toolbar”) should have been rejected, but my rubric accepted it. The repo itself was healthy, so every other check passed. Scope-bound needed to be rewritten. Condition (1) failed anything “listing sub-items”, so it read a detailed plan as an umbrella issue and The problem was the issue: opened by cursor[bot] (NONE), with no labels, Comments (0 total), and “Logo asset TBD.” it was a feature that the maintainer didn't agree to and my rubric didn't have a condition for it

**Evidence:** 
Before: condition (1) failed an issue for “listing sub-items”.
After: condition (1) only fails “an umbrella or tracking issue whose sub-items are meant to be split into separate issues or PRs”.
I added condition (6); a new user-facing feature that no maintainer has accepted, meaning no maintainer comment and no triage label and condition (7): an issue opened by a [bot] account with no maintainer follow-up.

```text
Run 1 (2026-09-29), first rubric:
agreement: 18/20 scored items  (bar: 18/20: PASS)
issue-01  accept  reject   NO
issue-20  reject  accept   NO

Run 3, saved as beat-1-sandbox/unit-1/eval-run.txt (2026-09-30T01:01:37Z):
agreement: 20/20 scored items  (bar: 18/20: PASS)
categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4
```

```text
scope-bounded, condition (1) after the fix (tools/issue-select/rubric.md, commit 17df116, 2026-09-29):
Fails if any of these is present: (1) the issue is an umbrella or tracking issue whose
sub-items are meant to be split into separate issues or PRs; ...

Unit 9 replacement row (installed ~/.claude/skills/issue-scout/rubric.md, 2026-11-15):
| scope-feasible | Issue body: the work it asks for | ... It fails only when there is no
single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a
container issue tracking many sub-tasks. An open design edge, an "etc.", or several files
involved never fails this check; that raises the difficulty note instead. | required |
```

Then ran a second check, a canary check for only issue 1 and issue 20, both were now fixed and finally ran a 3rd eval at 9:01pm and all accepted. 
Finally, I chose #64. That led to my claim on 2026-10-05 and, in the end, PR #83.

**plan-check**

**What it is, and the judgment hole I wrote:** 
plan-check reads a fix plan before you post it or build from it, and decides whether it’s ready: accept or reject. My rubric has 8 checks and 6 are required, these are required because these are the most important in checking whether a plan is ready or not.
1. cause-grounded: the cause the plan names fits the repro evidence, and none of the repro’s controls rule it out.
2. scope-bounded: every change is needed for the bug; no extra refactors.
3. executable: a stranger could start the work. The plan names where the change goes and commits to one approach.
4. test-decisive: the test names an outcome that would come out differently if the fix didn’t work.
5. thread-engaged: the plan follows, or answers, any maintainer direction or existing PR.
6. conventions-met: the plan comment meets the repo’s stated rules, such as AI disclosure.

**How it evolved, dated:** 
2026-10-12 (Unit 3):

Run 0: smoke test on the four calibration packages. All four agreed: agreement: 0/0 scored items (calibration packages aren’t scored).

Run 1: full run, one miss, not saved

```text
agreement: 18/19 scored items
pkg-02: claude exited 1: Warning: no stdin data received in 3s
pkg-14 (gold accept): failed: cause-grounded
```
pkg-14 (zellij#5174, colour strings leaking on SSH reattach) was a good plan, but it was rejected. The grader said the cache-clear control “points at the cache; the plan covers it only with an unevidenced claim.” My check said every control had to be “consistent with the cause”. The grader read “consistent” as “proven”.
Because pkg-02 errored, the harness wouldn’t save the run.

Change 1: cause-grounded loosened. I added:

A control the plan explicitly addresses with an explanation that nothing in the repro contradicts counts as consistent, even if that explanation is not yet proven; the plan does not have to prove its cause, only not be ruled out by its own evidence.

I rejected the other option, requiring proof of the cause, because “at the planning stage nobody has built the fix yet.”

Run 2: canaries (--only pkg-02,pkg-14,pkg-07,pkg-16,calib-03)

```text
agreement: 2/3 scored items
pkg-14: failed: executable
```
pkg-14 now passed cause-grounded, but it failed executable, because the plan says “exact functions to be pinned in the PR after tracing”.
The wrong-cause canaries pkg-07, pkg-16 and calib-03 still rejected, so loosening the check didn’t let bad plans through.
pkg-02 errored again.

Change 2: executable loosened. I added:

Naming the module and the code path is enough when the approach is fixed; leaving the exact function name to be pinned during the build does not fail this check.

pkg-14 names “the client attach/reattach path in zellij-server” and commits to one approach, so it’s buildable.

Fix for the environment: PYTHONUTF8=1. pkg-02 contains an emoji that Windows’ default encoding couldn’t pass to claude.

Run 3: canaries (--only pkg-02,pkg-14,pkg-10,pkg-17,pkg-18, with PYTHONUTF8=1)

```text
agreement: 5/5 scored items
```

pkg-02 ran, and pkg-14 was accepted. The unbuildable canaries pkg-10, pkg-17 and pkg-18 still rejected.

**Evidence:** 
This was the final run, Run 4 saved at 8:20 PM
The header of eval-run.txt reads written by run_eval.py at 2026-10-13T00:20:52Z.

```text
# eval run written by run_eval.py at 2026-10-13T00:20:52Z
agreement: 20/20 scored items  (bar: 18/20: PASS)
clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4
```

Real use: My plan for #64
8:21 PM: commits b652e3a (tool installed) and 7d9347d (eval run).
8:27 PM: commit bcf4014, “plan for issue #64 (checked with plan-check, accept)”.
The plan said the scorer was correct and the fixture was wrong. It changed only the test file: new query “Python Django REST API”, which should score 2/4 = 0.5, and the xfail marker removed.
The build matched the plan exactly. From plan-and-implement.md:
```text
before: 18 passed, 1 xfailed    avg_score=1.0
after:  19 passed               avg_score=0.5
unit suite: 375 passed, 53 xfailed → 376 passed, 52 xfailed
```
plan.md Deviations: “Nothing changed in the fix; the plan held.”
8:30–8:34 PM: commits e3b6caa and bd7262a, with the deviations, the build evidence and the posted plan comment (issuecomment-6135342026).
Unit 11 (2026-11-16): plan-check was used again on my Weblate plan. My Implementation Notes say “plan-check accepted the plan.”

**pr-precheck**

**What it is, and the judgment hole I wrote:** ...

**How it evolved, dated:** ...

**Evidence:** ...

## Inventory

- repro-check: ...
- (one bullet for every other tool, including any unit 9–12 tools)

## The Arc

**Beat 1**
Date: ...
Friction: ...
Change: ...
Result: ...

**Beat 2**
Date: ...
Friction: ...
Change: ...
Result: ...

(add Beat 3, Beat 4 … in the same shape, oldest first)

## Limitations and What's Next

...  (one or two paragraphs)

**Tool limits.**

issue-scout: [where this tool still stops, in plain words, or the word none]
pr-precheck: ...
plan-check: ...
issue-select: ...