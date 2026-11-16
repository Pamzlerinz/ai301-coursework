# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Kafilat Sarki-Umar

## Featured Tools

The tools I chose to write on are issue-select, plan-check and pr-precheck. The tools that changed the most were plan-check and pr-precheck. plan-check was changed because of cause-grounded which needed every control to be "consistent with the cause", the grader read "consistent" as "proven" and rejected pkg-14. And pr-precheck was changed because repro-rerun loosened after the smoke run. The one which did the most contribution to my Weblate contribution was pr-precheck. The other tools are less interesting is because these tools helped me with the final issue i picked


**issue-select**

**What it is, and the judgment hole I wrote:** 
I worked on the issue-select as the first project on this list. It checks information about an issue, checks if the repo is archived, who opens an issue, the labels, comments, pull requests, assignees and the repo's AI rules. An example of what I decided was unclear, which was a rule "Unclear Handling", a check that asks “is this alive and available?” needs proof, so unclear means fail. A check that asks “is there a problem here?” needs a red flag to fail, so unclear means pass. Its judgment is based on my `rubric.md`, which has 8 checks, of which 5 are required. The 5 required are required because these are the important aspects that need to be checked, namely;
1. maintainer-alive: checks if a human has committed or commented recently.
2. repo-in-use: checks if the repo isn’t archived and has been pushed to recently.
3. scope-bounded: checks if the issue is one clear, small task. It isn’t a huge umbrella issue, an open design debate, an unapproved new feature, or something a bot opened.
4. unclaimed: checks if nobody is assigned and nobody is already working on it.
5. ai-policy-allows: checks if the repo doesn’t ban AI-assisted contributions.

**How it evolved, dated:** 
2026-09-29: issue-select was downloaded, then utilizing AI, ran the eval and it worked for 18/20. Issue-01 and Issue-20 were missed. issue-01 (conda#16475) should have been accepted, but my rubric rejected it. It’s a single docs task whose “Proposed changes” section lists a new page plus updates to existing pages. issue-20 (excalidraw#11811, “Add company logo shape to the toolbar”) should have been rejected, but my rubric accepted it. The repo itself was healthy, so every other check passed. Scope-bound needed to be rewritten. Condition (1) failed anything “listing sub-items”, so it read a detailed plan as an umbrella issue, and the problem with issue-20 was: opened by cursor[bot] (NONE), with no labels, Comments (0 total), and “Logo asset TBD.” it was a feature that the maintainer didn't agree to and my rubric didn't have a condition for it.

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

**What it is, and the judgment hole I wrote:** 
pr-precheck reads a PR draft before you submit it: the diff, the description, the test evidence and the repo’s rules. It decides accept or reject. My rubric has 8 checks, and seven of them are required, each required check guards against something that would make a maintainer decline the PR, or that would mislead them:

1. plan-fidelity: every hunk in the diff is the planned fix, a test for it, a doc or changelog entry, or a change a deviation note explains.
2. description-honest: every claim in the title and description is true of the diff.
3. repro-rerun: the plan’s repro is shown re-run on the branch with an after-result you can see, and every failure mode the plan names is either shown or stated as not run.
4. repo-checks-run: the PR shows the repo’s own suite or linters were run, with their outcome.
5. diff-clean: no debug prints, commented-out code, stray files or formatting-only churn.
6. template-and-policy: the PR template and any AI policy are fully met.
7. thread-engaged: the PR follows maintainer direction, or explains why it differs.

**How it evolved, dated:** 
Smoke run on the 4 calibration packages (2026-10-19, Unit 4)

All four matched their labels. calib-01 was accepted, and calib-02, calib-03 and calib-04 were rejected. But one check read calib-03 wrongly:

calib-03’s after-result was “matches the issue’s expected output byte for byte”, and its control was “All-comments control unchanged”.
The grader failed repro-rerun, saying neither gave “an output line, exit code or value”.
That was wrong, because the issue writes out the expected output, so a reader can check it. And demanding full output for a control that already worked would push good PRs into rejects.

The change: repro-rerun loosened. I added two sentences:

An after-result that says it matches a specific expected output the plan or issue writes out (“matches the issue’s expected output byte for byte”) is observable. For a control (a case that already worked and must keep working), naming it and saying it is unchanged is enough.

I rejected the other option, accepting any stated after-result, because it would pass “should work” and “the library update looks right” (calib-02).

**Evidence:** 
I then did a full run, saved at 7:53 PM EDT on 2026-10-19

```text
# eval run written by run_eval.py at 2026-10-19T23:53:36Z
agreement: 20/20 scored items  (bar: 18/20: PASS)
clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3
```

not-tested 4/4 shows the loosening didn’t let untested PRs through, and calib-04 still failed on its missing second failure mode.

It changed less than my other tools did: one revision, and only one full run. 

## Inventory

- repro-check: It grades a reproduction and decides whether it really reproduces the issue with evidence. It went from 17/20 to 19/20 and was saved on 2026-10-05. One miss is still not resolved.
- voice guard: It is a hook that checks every comment, issue or PR body that I post through Claude Code against the voice-guide.md rules before it goes out, installed Nov 15, and stopped 2 real drafts, the Weblate claim and the update. For now it passed the same claim text a minute before stopping it, and it doesn't fire when gh is called by its full path.
- CLAUDE.local.md, the repo memory file: it is a private notes file in my Weblate clone that Claude Code reads every session. It imports Weblate's AGENTS.md and holds conventions. It is in use, first saying the widgets needed cairo, which was wrong for the current state of Weblate and I corrected it.
- ship-gate: a hook that runs before gh pr create. It runs my manifest's checks, then my pr-precheck. The first run stopped my PR (description-honest); the second passed; PR #22856 opened.
- PR monitor: reads my open PR (checks, reviews, quiet days) and drafts my next move into a file. It never posts. It is not installed yet. 


## The Arc
**Beat 1:** issue-select, issue-20
Date: 2026-09-29
Friction: Run 1 scored 18/20. My rubric accepted issue-20, a feature request opened by cursor[bot] that no maintainer had agreed to. It also rejected issue-01, a single docs task, as an “umbrella”. I noticed both in the disagreement lines of the eval output.
Change: rewrote scope-bounded. Condition (1) now means “meant to be split into separate issues or PRs”, and I added (6) unaccepted new feature and (7) bot-opened issue.
Result: the canary run scored 2/2 and the full run 20/20. I then used the tool to rank #64 first, and that’s the issue I fixed.

**Beat 2:** repro-check, the honest cannot-reproduce
Date: 2026-10-05
Friction: Run 1 scored 17/20, below the bar. claims-supported rejected good reports: pkg-03 over one prose side note, and pkg-09, an honest cannot-reproduce, for saying “I ran this 5 times” with one output.
Change: the check now backs only the main outcome. One artifact can stand for repeated runs, and hedged guesses are allowed.
Result: 19/20 PASS, and the reject categories held (no-evidence 4/4, wrong-target 4/4). But pkg-05 newly flipped to reject, and that’s still unexplained.

**Beat 3:** plan-check, the unproven control
Date: 2026-10-12
Friction: Run 1 rejected pkg-14, a good plan, with failed: cause-grounded. The grader read “consistent with the cause” as “proven”.
Change: I added that a control the plan explains, which nothing in the repro contradicts, counts as consistent.
Result: pkg-14 passed cause-grounded, but it then failed executable, which led to a second change. The wrong-cause canaries still rejected, and the final run scored 20/20.

**Beat 4:** plan-check, the Windows encoding error
Date: 2026-10-12
Friction: pkg-02 kept crashing the harness (claude exited 1: Warning: no stdin data received in 3s), so the full run couldn’t be saved.
Change: I ran with PYTHONUTF8=1, because pkg-02’s emoji broke Windows’ default encoding.
Result: the canary run scored 5/5, and the full 20/20 run saved.

**Beat 5:** pr-precheck on MY own PR #83 draft
Date: 2026-10-19
Friction: the live run flagged that my draft claimed mypy passed “as the Makefile runs it” without showing that output, and called a +1/−5 diff “the two-line change”.
Change: I added the Makefile-form mypy output and both pytest commands, and described the change exactly.
Result: the second run was clean, with no voice rules broken, and I opened PR #83.



## Limitations and What's Next
1. My tools can’t check claims about evidence they never see. The pr-precheck write-up says it directly: “My tool can’t catch a confident false claim about evidence it never sees.” plan-check has the same gap, since a story that sounds right passes as long as nothing in the repro contradicts it. Every tool reads text, and none of them re-runs anything.
2. Each time I loosened a check, I accepted a new miss. repro-check, plan-check and pr-precheck were all loosened to stop rejecting good work, and each now lets through something it used to catch. I wrote down each of those trade-offs.

**What's Next.**
1. Re-run the evidence instead of trusting it
The limit it fixes: pr-precheck and plan-check only read text, so a false “matches byte for byte” claim passes.
What I’d build: a step that runs the repro command from the plan on the branch and compares the output itself.
Why it’s realistic: I already do this by hand. My #64 and Weblate test-evidence blocks are exactly those commands.

2. Re-check “unclaimed” right before claiming
The limit it fixes: issue-scout’s read went stale in 27 minutes on power-grid-model-io#107.
What I’d build: a last-minute check for linked PRs and new claim comments that runs just before my post.
Why it’s realistic: I already did this by hand on Weblate, with the GraphQL query that found PR #20392. The next step is making it part of the tool.

**Tool limits.**
issue-scout: its unclaimed check is only a snapshot: it graded power-grid-model-io#107 as free, but PR #540 was opened 27 minutes before my claim, and it couldn't see the sidebar-linked PR on #11161.
issue-select: It can’t know my skills; it only uses scope.md to rank issues it has already accepted.
plan-check: A plan that explains away a control with a believable but wrong story still passes cause-grounded.
pr-precheck: It passes a false claim like “matches the expected output byte for byte” or “control unchanged”, because it can’t re-run anything.