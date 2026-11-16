# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Kafilat Sarki-Umar

## Featured Tools

The tools I chose to write on are issue-select, plan-check and pr-precheck. The tools that changed the most were plan-check and pr-precheck. plan-check was changed because of cause-grounded which needed every control to be "consistent with the cause", the grader read "consistent" as "proven" and rejected pkg-14. And pr-precheck was changed because repro-rerun loosened after the smoke run. The one which did the most contribution to my Weblate contribution was pr-precheck. The other tools are less interesting is because these tools helped me with the final issue i picked


**issue-select**

**What it is, and the judgment hole I wrote:** 
I worked on the issue-select as the first project on this list. It checks information about an issue, checks if the repo is archived, who opens an issue, the labels, comments, pull requests, assignees and the repo's AI rules. Its judgment is based on my `rubric.md`, which has 8 checks, of which 5 are required. The 5 required are required because these are the important aspects that need to be checked, namely;
1. maintainer-alive: checks if a human has committed or commented recently.
2. repo-in-use: checks if the repo isn’t archived and has been pushed to recently.
3. scope-bounded: checks if the issue is one clear, small task. It isn’t a huge umbrella issue, an open design debate, an unapproved new feature, or something a bot opened.
4. unclaimed: checks if nobody is assigned and nobody is already working on it.
5. ai-policy-allows: checks if the repo doesn’t ban AI-assisted contributions.

**How it evolved, dated:** 
2026-09-29: issue-select was downloaded, then utilizing AI, ran the eval and it worked for 18/20. Issue-01 and Issue-20 were missed. issue-01 (conda#16475) should have been accepted, but your rubric rejected it. It’s a single docs task whose “Proposed changes” section lists a new page plus updates to existing pages. issue-20 (excalidraw#11811, “Add company logo shape to the toolbar”) should have been rejected, but your rubric accepted it. The repo itself was healthy, so every other check passed. Scope-bound needed to be rewritten. Condition (1) failed anything “listing sub-items”, so it read a detailed plan as an umbrella issue and The problem was the issue: opened by cursor[bot] (NONE), with no labels, Comments (0 total), and “Logo asset TBD.” it was a feature that the maintainer didn't agree to and my rubric didn't have a condition for it

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
Finally, I chose #64. That led to your claim on 2026-10-05 and, in the end, PR #83.

**plan-check**

**What it is, and the judgment hole I wrote:** ...

**How it evolved, dated:** ...

**Evidence:** ...

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