---
name: delegate
description: Delegate work to subagents. Use for tasks that may fit a specialist domain. Covers discovery, selection, structured handoff, subdelegation, and review.
---

# Delegate

Use this skill to discover specialists, select the best fit, delegate with a structured handoff, and review delegated results.

This skill is protocol-first. Do not frame it around role identity.

---

## Your Tools

- Subagent invocation tool — dispatch focused work to a subagent
- Read/View tool — verify output

Everything else belongs to the delegated agent unless task constraints require otherwise.

---

## When This Skill Activates

Use this skill when:
- a task may fit a specialist domain
- implementation, investigation, or multi-step execution is needed
- multiple specialists may be needed for independent scopes
- you are deciding whether generalist fallback is justified

Do not skip this skill because the current agent seems capable enough. Broad capability is not a reason to bypass specialist selection.

---

## Discovery Workflow

Before dispatching, inspect all agent sources exposed by the current environment.

Discovery rules:
- consider all supported sources in the current platform context
- include workspace-scoped sources when available
- include user or global sources when available
- tolerate platform differences in source discovery
- ignore invalid, incomplete, or incompatible candidates
- continue safely if some sources are unavailable
- if complete discovery is not observable, use the best supported path and state that limitation before fallback

Collect at least:
- agent identifier
- description
- declared specialization or capability summary
- relevant scope or constraints when available

Do not hard-code one platform's paths, commands, or installation assumptions into the normative policy.

---

## Selection Workflow

Select by best semantic match.

Selection rules:
- match the task against description, specialization, scope, and constraints
- prefer the most semantically specific eligible specialist
- prefer specialists over generalists whenever an eligible specialist exists
- allow multiple specialists when the task decomposes into distinct scopes
- use stable documented tie-break rules when candidates are similarly suitable
- reject "current agent can also do it" as a selection argument

Eligibility minimum:
- explicit domain or task match in the description
- no conflict with stated constraints
- scope compatible with the immediate task
- no clearly better eligible specialist

Tie-break rules:
- first: semantic specificity to the immediate task
- second: stable documented precedence in the current environment
- third: split into multiple delegations when scopes are complementary

When many candidates exist, select the best fit, not the most convenient fit.

---

## Delegation Workflow

Delegation is the default path for specialist work. If an eligible specialist exists, delegation is required.

Every handoff should include:
- task
- objective
- scope
- constraints
- relevant files or context
- expected deliverable
- expected return format

Handoff rules:
- provide explicit context
- keep context isolated
- do not rely on hidden session state
- do not say "see plan above"
- provide enough information to avoid avoidable `NEEDS_CONTEXT`
- do not mix review responsibility with ambiguous execution ownership

### Canonical Handoff Shape

```text
Task: <what to do>
Objective: <why this work is needed>
Scope: <what is in and out>
Constraints: <rules, limits, expectations>
Relevant files or context: <paths, snippets, facts>
Expected deliverable: <artifact or decision>
Expected return format: <status + content>
```

---

## Parallel Delegation

Use parallel delegation only when scopes are non-overlapping and integration dependencies are absent or explicitly planned.

Good candidates:
- different files or subsystems
- separate evidence gathering
- independent validation or analysis
- complementary specialists with distinct responsibilities

---

## Generalist Fallback

Generalist fallback is allowed only when specialist delegation is not justified under the current task.

Fallback rules:
- no eligible specialist exists, or all candidates are unsuitable
- or delegation is explicitly constrained by the user
- or delegation failed and fallback is allowed by the current task constraints
- explain briefly why fallback applies
- keep justification factual
- identify the failed discovery, selection, or constraint condition
- never use "good enough" as justification
- never fall back silently

Fallback mode:
- prefer delegating to a generalist agent when the environment supports it
- direct execution by the current agent is allowed only when delegation is constrained, unavailable, or already failed under the current task constraints

---

## Subdelegation Rules

Subdelegation is allowed by policy.

Use it when:
- a narrower specialist is a better fit for a subtask
- decomposition improves quality or speed
- independent subtasks benefit from further specialist separation

Subdelegation rules:
- the delegating agent remains accountable
- review subdelegated output before integrating it
- avoid loops and ping-pong
- avoid uncontrolled fan-out
- preserve explicit scope ownership
- do not subdelegate to avoid synthesis or review responsibility

Do not subdelegate by habit. Subdelegate because specialist fit improves.

---

## Review And Synthesis

After each delegated result returns:

### Stage 1: Conformance

Did it deliver what was requested? Nothing more, nothing less?

### Stage 2: Quality

Is it correct, maintainable, and usable for the current task?

If multiple delegated outputs return:
- compare them against the assigned scopes
- resolve conflicts
- synthesize a single coherent result before returning upward

Do not pass through raw delegated output without review.

---

## Status Protocol

Delegated agents report one of:

| Status | Meaning | Action |
|---|---|---|
| `DONE` | Completed, ready for review | Proceed to review |
| `DONE_WITH_CONCERNS` | Completed but flagged issues | Read concerns first |
| `NEEDS_CONTEXT` | Missing information | Re-dispatch with more context |
| `BLOCKED` | Cannot complete | Assess context, break task, or escalate |

This protocol applies across the delegation tree, including subdelegated work.
If `NEEDS_CONTEXT` or `BLOCKED` repeats without material progress, stop redispatching, declare the blocker, and choose among: provide missing context, decompose differently, escalate, or apply justified fallback.

---

## Gotchas

- skipping discovery causes premature fallback
- picking a generalist too early weakens the workflow
- handoffs without explicit structure increase `NEEDS_CONTEXT`
- delegation never removes accountability from the delegating agent
- overlapping scopes across multiple agents create integration risk
- platform-specific discovery details belong in platform adapters, not core policy
