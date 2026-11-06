---
name: council
description: Trigger this if the user says 'council'.
---

# Council

Before an audit, address outstanding PR comments, fix known issues, and run
relevant checks. Give council the updated implementation; do not defer known
fixes until reviews return. If the user wants to discard the implementation
and explore new ideas, skip fixes to code being replaced.

Bring in two delegates to help with the user's request, usually for review
or feedback. The council has exactly these two members:

| Delegate | Provider | Model | Reasoning effort |
| --- | --- | --- | --- |
| Claude Opus | Claude (`claudeAgent`) | `claude-opus-5-5` | `medium` (`effort`) |
| GPT Sol | Codex (`codex`) | `gpt-6.1-sol` | `xhigh` (`reasoningEffort`) |

Do not add, swap, or substitute members, and do not change their effort
levels, unless the user names different council members for this request.

In T3 Code, use `orchestrator_capabilities` to confirm both models, then
`delegate_task` to start one delegate per model in parallel, passing the
effort above in `target.options`. Elsewhere, use the harness's subagent
tools. If a member or its effort level is unavailable, report it instead of
substituting.

Give each delegate the request and relevant context. For review or feedback,
keep their work read-only. Collect their responses and summarize the useful
findings, disagreements, and recommended next steps.

For a code review, ask each delegate for a score out of 10.
Count each full review pass as one round.
A fresh review after fixes starts a new round.

If the work ends in a pull request, put the results in the PR description.
Put this section before the model and harness line at the end:

```markdown
## Council review

Rounds: 2

| Delegate | Round 1 | Round 2 |
| --- | --- | --- |
| Claude Opus | 7/10 | 9/10 |
| GPT Sol | 8/10 | 10/10 |
```

If a delegate gives no score, write `no score`. Do not invent one.
If the PR is open, edit the description. Do not post a comment for this.
If there is no PR, put the same results in the final reply only.

If you agree with their recommendations, make the changes and request a fresh
review after. Continue this process until both delegates are satisfied with the
outcome.

Do not let repeated reviews result in scope creep. It is your job to fight to
keep scope limited.
