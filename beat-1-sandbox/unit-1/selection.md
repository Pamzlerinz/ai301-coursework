# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64

**Verdict output**

```
Scope: all three are in codepath/pathreview-ai301-fa26-howard, so they're in scope. Evidence was gathered on 2026-09-29 from the public GitHub API, since gh isn't installed here.

Repo-level facts (same for all three):
- Recent commits: the last 5 are by a human (Aburke225), the newest on 2026-09-16.
- Archived: no. The last push was 2026-09-16, and there are no releases.
- AI policy: docs/CONTRIBUTING.md (189 lines) and the PR template don't mention AI, which counts as a pass.
- Maintainer response times: none of the recent non-maintainer comments or issues has a reply from an owner, member or collaborator yet. That covers the claim on #72 and the external issue #76 (both 9/18, 11 days with no reply) and the claim on #54. With no first responses to measure, a median can't be computed, so fast-response is unclear.

Ranked read-out (all three accepted):

1. #64: Relevance scorer "partial overlap" test fixture has full overlap. Best fit. It's pure Python test work with a one-command repro (pytest tests/unit/test_relevance_scorer.py -q shows assert 1.0 < 0.9). That lines up with your goals of reproducing bugs and writing tests, and it's also the smallest change.
2. #26: Safety event count in the /health endpoint. Good fit. It's a 2–4 hour Python change that has you read two unfamiliar modules (api/routes/health.py, safety/monitoring.py) and join them up, and the new value is easy to test.
3. #15: Agent session state not cleared between reviews. Fits, but ranks lower. It spans three agent files, and resetting the orchestrator's lifecycle is closer to the structural changes you said you'd rather avoid.

The three tie on preferred checks: each passes maintainer-endorsed (good first issue label, opened by a collaborator), fails maintainer-in-thread (no comments at all) and is unclear on fast-response. So fit alone sets the order.

Something the rubric doesn't catch: none of the claim comments on sibling issues has had a maintainer reply. Review of your PR may be slow, whichever issue you pick.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Human commits by Aburke225 on default branch, newest 2026-09-16 (13 days before 2026-09-29)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; last push 2026-09-16, within 180 days; no releases"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single bug: 'Fix the fixture so the overlap is genuinely partial'; opened by COLLABORATOR with bug/good first issue labels; no debate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no cross-referenced or linked PRs; none of the repo's 4 PRs mention #64"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI policy (silence passes)"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Label 'good first issue'; opener Aburke225 is COLLABORATOR"},
      {"name": "fast-response", "grade": "unclear", "evidence": "No maintainer first responses in sample (#72, #76, #54 non-maintainer comments from 9/18 still unanswered); median not computable"},
      {"name": "maintainer-in-thread", "grade": "fail", "evidence": "Issue has zero comments"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/26",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Human commits by Aburke225 on default branch, newest 2026-09-16"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; last push 2026-09-16, within 180 days; no releases"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Bounded bug: /health returns literal 0 for safety_events_last_hour; 2 named files, est. 2-4 hours; opened by COLLABORATOR with triage labels"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no cross-referenced or linked PRs; none of the repo's 4 PRs mention #26"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI policy (silence passes)"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Label 'good first issue'; opener Aburke225 is COLLABORATOR"},
      {"name": "fast-response", "grade": "unclear", "evidence": "No maintainer first responses in sample; median not computable"},
      {"name": "maintainer-in-thread", "grade": "fail", "evidence": "Issue has zero comments"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/15",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Human commits by Aburke225 on default branch, newest 2026-09-16"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; last push 2026-09-16, within 180 days; no releases"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "One coherent fix: 'Empty that cache and delete the profile's saved session state when run starts'; 3 named files, est. 3-4 hours; opened by COLLABORATOR"},
      {"name": "unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no cross-referenced or linked PRs; none of the repo's 4 PRs mention #15"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI policy (silence passes)"},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Label 'good first issue'; opener Aburke225 is COLLABORATOR"},
      {"name": "fast-response", "grade": "unclear", "evidence": "No maintainer first responses in sample; median not computable"},
      {"name": "maintainer-in-thread", "grade": "fail", "evidence": "Issue has zero comments"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

1. Run 1, full run, first rubric: `agreement: 18/20 scored items  (bar: 18/20: PASS)`. Disagreements: `issue-01  accept  reject   NO` and `issue-20  reject  accept   NO`. Category tallies: `claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1  scope 3/4`.
2. Run 2, partial run (`--only issue-01,issue-20`) after revising the scope-bounded check: `agreement: 2/2 scored items`.
3. Run 3, full run saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Category tallies: `claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.

**Issue analysis**

issue-20 (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar"). Gold label: reject. My rubric's decision in Run 1: accept (`issue-20  reject  accept   NO     graded accept`). In Run 3, after revising the rubric: reject.

Why Run 1 accepted it: every signal outside the issue text is healthy. The repo facts show `last push to any branch: 2026-08-04`, five human commits on 2026-08-04, `archived: no`, a maintainer first response of `0.6 days`, `contribution policy (CONTRIBUTING.md): no statement on AI or contribution tooling`, and `assignees: none; linked PRs: none`. So maintainer-alive, repo-in-use, unclaimed and ai-policy-allows all passed. My original scope-bounded check only failed issues that were umbrellas, design debates, core-internals changes, usage questions, or old with several abandoned PRs. This issue is none of those, so it passed and the verdict was accept.

Why the gold label is reject: the problem is in who asked and whether anyone agreed. The bundle says `opened by cursor[bot] (NONE) on 2026-08-02, state open, labels: none`, and `Comments (0 total, first 0 shown)`. It asks for "our company logo" as a built-in toolbar shape, with `Logo asset TBD.` It is a new-feature request filed by an AI agent, for one requester's private need, and no maintainer has commented, labeled or accepted it. A newcomer who built it would likely have the PR declined because the feature itself was never wanted. My rubric had no condition for an unaccepted new feature or a bot-filed issue, so it could not see this.

**Check rationale**

Check `scope-bounded` (weight: required), pass condition as currently written in `tools/issue-select/rubric.md`:

> Fails if any of these is present: (1) the issue is an umbrella or tracking issue whose sub-items are meant to be split into separate issues or PRs; (2) the thread shows an open design debate with no maintainer decision; (3) a maintainer states the fix requires changes to core internals; (4) it is a usage or support question, not a requested change; (5) open more than 2 years with 2 or more closed, unmerged PRs; (6) it requests new user-facing functionality (a new tool, element type, or setting) and no maintainer has accepted it: no maintainer comment and no triage label; (7) it was opened by a bot account (name ending in `[bot]`) with no maintainer follow-up. Otherwise passes. These are not fails: a short body, a checklist, missing repro steps, or a long, detailed proposal for one coherent change, even one that touches several files of the same kind (for example several docs pages).

Why it has this form: the check is a list of explicit fail signals, and anything else passes, because the evidence guide warns that "short is not the same as unscoped." Two changes came from Run 1. Condition (1) used to say only "listing sub-items," and it rejected issue-01 (conda#16475), a single docs task whose "Proposed changes" section lists one new page plus updates to existing pages. Adding "meant to be split into separate issues or PRs," plus the closing sentence about one coherent change touching several docs pages, stops a detailed spec from reading as an umbrella. Conditions (6) and (7) were added for issue-20: a new feature nobody on the project accepted, filed by a bot, is not a bounded task a newcomer can land.

**Trade-offs**

Condition (6) gives up genuine community feature requests that maintainers would welcome but have not triaged yet: an unlabeled, uncommented request for a new setting will be rejected even if it is a good idea. Condition (7) gives up legitimate issues opened by automation, such as bot-filed bug reports, unless a maintainer has followed up. I accept both misses, because an unaccepted feature is a risky first contribution. To check that the change did not break anything else, I re-ran the two changed cases as canaries with `--only issue-01,issue-20` (`agreement: 2/2 scored items`), then ran all 20 again. The 18 issues that already agreed in Run 1 still agreed in Run 3 (`agreement: 20/20 scored items`).

---

## Selection rationale

**Selection rationale**

1. It's test work I want to practice, and it's small enough for the time I had to work on it.
2. The skills correctly saw that #64 had a "good first issue" label and was opened by a collaborator.
3. It wasn't difficult to claim but the maintainers haven't replied to the recent claim comments.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.