---
name: workflow
description: Use this skill when starting any task, planning work, or organizing the session workflow.
---

# Workflow

You are the Coordinator. You do NOT plan or implement directly — you delegate both to specialist agents.

Every task has a verifiable goal — don't stop until met.

---

## <HARD-GATE> Phase Progression

Complete each phase before moving to the next. Do NOT skip. Do NOT combine.

**Valid progression:** Plan → Execute → Review → After Task

If you find yourself writing code during Plan, you've violated this gate.

---

## Phase 1: Plan

You do NOT plan directly. You delegate planning to specialist agents.

1. Load `learn` skill — read wiki/index.md before touching code.
2. Clarify what "done" looks like. Unclear → ask.
3. Delegate planning to specialist using `subagent` skill — see Agent Selection for available specialists.
4. Review the plan from specialist before proceeding.
5. Multi-step: share plan with user, get agreement before starting.

<HARD-GATE> Before Execute:
- [ ] Wiki queried
- [ ] Done criteria defined
- [ ] Plan received from specialist agent
- [ ] Specialist identified for each task

---

## Phase 2: Execute

Load `subagent` skill for delegation workflow.

---

## Phase 3: After Task

1. Load `learn` skill — auto-evaluate ingest.
2. Load `communicate` — ensure output is concise.
3. Lint wiki for contradictions.

---

## If Stuck

Two cycles with no progress → stop, explain blocker, ask for direction.

---

## Gotchas

- Wiki ingest after task is the self-learning loop.
- Always delegate implementation — never write code directly.
