---
name: workflow
description: Use this skill when starting any task with multiple steps or unclear success criteria. Start by loading wiki skill to query existing knowledge. Delegate planning and coordination to an orchestrator agent, which dispatches specialist agents in parallel when possible. Activates when task requires planning, implementing, or verifying work.
---

# Workflow

Every task has a verifiable goal — don't stop until met. Flow: wiki query → plan → delegate → verify → ingest.

---

## <HARD-GATE> Phase Progression

Complete each phase before moving to the next. Do NOT skip. Do NOT combine.

**Valid progression:** Plan → Execute → Verify → After Task

If you find yourself writing code during Plan, you've violated this gate.

---

## Phase 1: Plan

1. Load `wiki` skill — read `wiki/index.md` first, then relevant pages before touching code.
2. Load `invoke-subagents` skill — you orchestrate, specialists execute. Applies to planning too.
3. Load `think-before-acting` skill — validate understanding.
4. Define what "done" looks like. Unclear → ask.
5. Multi-step tasks: share plan, get agreement before starting.

<HARD-GATE> You may NOT proceed to Execute without:
- [ ] Wiki queried
- [ ] `invoke-subagents` skill loaded
- [ ] Understanding validated
- [ ] "Done" criteria defined
- [ ] Plan shared and agreed upon
- [ ] Specialist(s) identified for each task

---

## Phase 2: Execute

1. Multi-task plans: load `parallel-work` skill — dispatch independent tasks to subagents in parallel.
2. Sequential tasks: invoke specialist subagent per task with fresh context.
3. Each subagent gets a focused, self-contained prompt. Never inherits your session.
4. Final validation is your responsibility — review each output before presenting.

<HARD-GATE> The first tool call in Execute phase MUST be a `task` invocation. If you call Edit, Write, or Bash (for implementation) before calling `task`, you've violated this gate. Reading files to build a specialist prompt is allowed; implementing is not.

<HARD-GATE> Every implementation task MUST be delegated. You orchestrate, review, validate — you do not implement.

---

## Phase 3: Verify

1. Run tests, linter, build. Fix failures first.
2. Two-stage review per task: spec compliance → code quality.
3. Show diff of what changed.
4. Wait for explicit approval — any clear affirmative counts.

<HARD-GATE> You may NOT claim completion without:
- [ ] Tests/linter/build passing
- [ ] Spec compliance verified
- [ ] Code quality verified
- [ ] Diff shown to user
- [ ] Explicit approval received

---

## Phase 4: After Task

1. Load `wiki` skill — ingest what this task taught. **Evaluate automatically** — do NOT wait for the user.
2. Load `skill-candidates` skill — evaluate if this task revealed a recurring procedural pattern.
3. Lint wiki for contradictions or stale claims.

<HARD-GATE> You may NOT end a task without:
- [ ] Evaluating whether wiki ingest is needed
- [ ] Evaluating whether a procedural pattern could become a skill candidate
- [ ] Linting wiki for contradictions

---

## If Stuck

Two cycles with no progress → stop, explain the blocker, ask for direction. Being stuck does NOT exempt you from the workflow.

---

## Rationalization Prevention

Excuses agents use to skip phases. None are valid.

| Excuse | Reality |
|--------|---------|
| "Too simple for a workflow" | Simple things become complex. Follow the flow. |
| "I can do this faster myself" | Delegation takes 30 seconds. Fixing your mistakes takes 10 minutes. |
| "I already know how to do this" | Knowing how ≠ doing it right. Fresh context catches what you miss. |
| "I'll plan while I implement" | Plan and Execute are separate phases. Do not combine. |
| "No need to update wiki for this" | Evaluate automatically. Don't pre-judge. |
| "This pattern isn't worth tracking" | That's for `skill-candidates` to decide. |
| "I'll verify after I finish everything" | Verify after each subagent. End-of-pipeline verification catches cascading failures too late. |
| "The user is waiting, let me rush" | Presenting broken work wastes more time than verifying first. |

---

## Gotchas

- "Looks good" counts as approval — don't ask again after receiving it.
- Presenting results with failing QA is worse than not presenting — fix first.
- Wiki ingest and skill candidate evaluation at task end are not optional — they're the self-learning loop.
