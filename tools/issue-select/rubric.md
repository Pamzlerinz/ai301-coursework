# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: "last 5 default-branch commits" (author and date). Comments section: each comment's author_association and date. | At least one of: (a) a commit by a human author, or a bot commit that merges a human's PR, dated within 90 days of the capture date (today in live mode); (b) a comment by an OWNER, MEMBER, or COLLABORATOR in this issue's thread within 90 days. Commits by `[bot]` authors alone do not count. | required |
| repo-in-use | Repo line: "archived:". Repo facts: "last push to any branch", "latest release". | Fails if archived is true. Otherwise passes if the last push to any branch is within 180 days of the capture date, or the latest release is within 365 days. A repo with no releases is judged on last push alone. | required |
| scope-bounded | Issue body, labels, opener (author and author_association), and full comment thread. | Fails if any of these is present: (1) the issue is an umbrella or tracking issue whose sub-items are meant to be split into separate issues or PRs; (2) the thread shows an open design debate with no maintainer decision; (3) a maintainer states the fix requires changes to core internals; (4) it is a usage or support question, not a requested change; (5) open more than 2 years with 2 or more closed, unmerged PRs; (6) it requests new user-facing functionality (a new tool, element type, or setting) and no maintainer has accepted it: no maintainer comment and no triage label; (7) it was opened by a bot account (name ending in `[bot]`) with no maintainer follow-up. Otherwise passes. These are not fails: a short body, a checklist, missing repro steps, or a long, detailed proposal for one coherent change, even one that touches several files of the same kind (for example several docs pages). | required |
| unclaimed | Repo facts: "this issue: assignees:", "linked PRs:" with state. Comments section: claim phrases ("I'll take this", "can I work on this", "working on this") and PR mentions. | Fails if any of: (1) the issue has an assignee; (2) any open PR is linked in the sidebar or mentioned in the thread; (3) a claim comment was posted within 30 days of the capture date. Closed, unmerged PRs are abandoned attempts and do not fail this check. If the sidebar and thread disagree, the thread wins. | required |
| ai-policy-allows | Repo facts: "contribution policy" line (CONTRIBUTING.md, AI policy files, templates). | Fails only if the policy explicitly bans AI-generated or AI-assisted contributions. Conditions (disclosure, testing, personal understanding, human review) pass. No policy passes. | required |
| maintainer-endorsed | Issue labels; author_association of the issue opener. | Has a "good first issue" or "help wanted" label, or was opened by an OWNER, MEMBER, or COLLABORATOR. | preferred |
| fast-response | Repo facts: "maintainer first-response sample". | The median first maintainer response in the sample is 14 days or less. | preferred |
| maintainer-in-thread | Comments section: author_association and date. | A maintainer (OWNER, MEMBER, COLLABORATOR) commented in this issue within 30 days of the capture date. | preferred |

## Verdict rule

Accept if every required check passes; reject if any required check fails.
Preferred checks never change the verdict; they rank accepted issues, with
more preferred passes ranking higher.

Unclear handling: for maintainer-alive, repo-in-use, and unclaimed, unclear
counts as fail (missing evidence of life, or of availability, is a risk).
For scope-bounded and ai-policy-allows, unclear counts as pass, because
those checks fail only on an explicit signal in the evidence.