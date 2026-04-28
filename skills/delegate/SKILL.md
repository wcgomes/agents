---
name: delegate
description: Delegate work to subagents. Use for ANY task — delegating focused work to a subagent is the default expected behavior. Covers agent discovery, context isolation, dispatch workflow, and status handling.
---

# Delegate

Delegate work to focused subagents. This skill is for Coordinators.

---

## Your Tools

- Subagent invocation tool — dispatch a focused task to a subagent
- Read/View tool — verify output

Everything else (Edit, Write, Bash/Shell) belongs to subagents.

---

## When to Delegate

**Always delegate** when task involves:
- Implementation code
- Investigation or research
- Multi-step work
- Any work that fits a specialist domain

**Never** write implementation code directly.

---

## Agent Discovery

Before dispatching, check if specialized agents are available in your environment.

1. **List available agents** — check workspace and global agent directories
2. **Read descriptions** — match task domain to agent specialty
3. **Select best fit** — dispatch to the specialist whose description aligns with the task
4. **Fallback** — if no specialist matches, use the default/general subagent

Specialized agents are defined as Markdown files with frontmatter (`name`, `description`). Read the `description` field to determine fit.

---

## Context Isolation

Subagents never inherit session context. Provide:
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

Issues found → fix → re-review until approved.

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

When multiple tasks are independent (different files):
1. Group by file scope
2. Dispatch one subagent per task
3. All run concurrently
4. Integrate results after

Same domain is fine — same files is not.

---

## Gotchas

- Subagents should NEVER inherit session context
- Always provide file paths explicitly; never say "see plan above"
- Multiple subagents on same files = conflict risk
