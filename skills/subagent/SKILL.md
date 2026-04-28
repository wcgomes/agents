---
name: subagent
description: Use this skill when planning or executing tasks. The primary skill for orchestrating agent work.
---

# Subagent

You orchestrate; specialist subagents execute. This skill handles delegation workflow.

---

## Your Role

You have two tools:
- `task` — invoke a specialist agent
- `read` — verify output

Everything else (Edit, Write, Bash) belongs to specialist subagents.

---

## When to Delegate

**Always delegate** when task involves:
- Implementation code
- Investigation or research
- Multi-step work
- Any work that fits a specialist domain

**Never** write implementation code directly.

---

## Agent Selection (184+ agents)

**Domain prefixes:**

| Task Type | Prefix | Example Agents |
|---|---|---|
| Code, API, infra, DB | `engineering-` | frontend-developer, backend-architect, sre |
| Content, social, ads | `marketing-` | growth-hacker, seo-specialist, tiktok-strategist |
| Sales, proposals | `sales-` | outbound-strategist, proposal-strategist |
| UI, UX, visual | `design-` | ui-designer, ux-researcher |
| QA, testing | `testing-` | api-tester, performance-benchmarker |
| Product, roadmap | `product-` | sprint-prioritizer, feedback-synthesizer |
| Project coordination | `project-management-` | studio-producer, experiment-tracker |
| Domain-specific | `specialized-` | legal-compliance-checker, healthcare-customer-service |

**Selection heuristics:**
1. Match task keywords → agent description
2. Default to `general` if unsure
3. Escalate: first attempt didn't fit → try another in same domain

Invoke by `@domain-specialist` (e.g., `@frontend-developer`).

---

## Delegation Flow

1. Select specialist by domain prefix (see Agent Selection above)
2. Build isolated context — subagent never inherits your session
3. Dispatch with focused prompt
4. Review result before proceeding

---

## Context Isolation

Subagents should NEVER inherit session context. Provide:
- Exact task description
- Relevant scope and constraints
- Expected output format
- File paths if needed

Do NOT make subagent read plan files — provide full text.

---

## Two-Stage Review

After each subagent returns:

### Stage 1: Conformance
Did they build what was requested? Nothing more, nothing less?

### Stage 2: Quality
Is it well-built? Clean, tested, maintainable?

**Review loops:** Issues found → fix → re-review until approved.

---

## Status Protocol

Subagents report one of:

| Status | Meaning | Action |
|---|---|---|
| `DONE` | Completed, ready for review | Proceed to review |
| `DONE_WITH_CONCERNS` | Completed but flagged issues | Read concerns first |
| `NEEDS_CONTEXT` | Missing information | Re-dispatch with context |
| `BLOCKED` | Cannot complete | Assess: more context OR break task OR escalate |

---

## Parallel Dispatch

When multiple tasks are **independent** (different files):

1. Group by file scope
2. Dispatch one specialist per file
3. All run concurrently
4. Integrate results after

Same domain is fine — same files is not.
