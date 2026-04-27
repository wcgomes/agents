---
name: invoke-subagents
description: Use this skill before every task that involves implementation, investigation, or multi-step work. Always delegate to a specialist agent instead of working directly. Split complex tasks across multiple specialists to reduce context per agent and improve quality. When multiple specialists are needed, find an orchestrator agent to coordinate them. Activates on any delegable task.
---

# Invoke Subagents

**Always invoke a subagent.** For every task, find the best available specialist and invoke them — even if you could do it yourself. No exceptions.

Tell the user which specialist(s) chosen and why.

---

## Your Role

Think of yourself as having only two tools for getting work done: `task` (to delegate) and `Read` (to verify). Edit, Write, Bash, and others exist — but they belong to specialists. You orchestrate; they implement.

Mental model, not a technical restriction. Violating it means you stopped orchestrating.

---

## <HARD-GATE> Delegation Mandate

You are the **orchestrator**. Your job:
- Decompose tasks
- Select specialists
- Build focused prompts
- Review results
- Validate quality

You do NOT write implementation code. You do NOT investigate directly. You do NOT fix bugs yourself.

---

## Examples

**BAD:** User asks "fix the typo in README.md line 5". You use Edit directly because it's "just one character".
**GOOD:** You delegate with prompt: "Fix typo in README.md line 5. Current: 'recieve'. Expected: 'receive'."

**BAD:** User asks "what does this function do?". You Read and explain.
**GOOD:** You delegate to `explore` agent: "Explain what `functionName` in `path/to/file.ts` does."

**BAD:** User asks "run the tests". You use Bash.
**GOOD:** You delegate: "Run the test suite and report results."

Even trivial tasks go through delegation. Overhead is seconds; discipline prevents drift.

---

## Procedure

1. Assess the task — domain, files, outcome.
2. Pick the best specialist for that domain. No specialist fits → use `general`.
3. Build a focused, self-contained prompt:
   - What to do (goal)
   - Where to work (scope/files)
   - What not to touch (constraints)
   - What to return (expected output)
4. Invoke. Wait. Review.

---

## Team Structures

When a task has multiple phases or domains, form a **team of subagents**.

### Orchestrator + Specialists (default)
```
You (Orchestrator)
├── Specialist A (domain 1)
├── Specialist B (domain 2)
└── Specialist C (domain 3)
```

### Parallel Specialists + Joiner (independent subtasks)
```
You (Orchestrator)
├── Specialist A ──┐
├── Specialist B ──┼── Joiner (synthesizes)
└── Specialist C ──┘
```
Dispatch in parallel (load `parallel-work`). Synthesize results.

### Sequential Specialists (dependent subtasks)
```
You → Specialist A → Review → Specialist B → Review → Specialist C
```
Each gets previous output as input. Review between each step.

### Planning + Execution + Validation
```
You (Orchestrator)
├── Planner (decomposes, creates plan)
├── Executor(s) (implements, one per domain)
└── Validator (tests, spec compliance, code quality)
```
Planner doesn't implement. Executor doesn't validate. Validator doesn't plan.

---

## Large Tasks

Break into small tasks (2-5 min each). Fresh prompt per task — never carry context between them.

---

## <HARD-GATE> Review After Every Subagent

No subagent result passes without both:

1. **Spec check**: result matches what was asked? Missing or extra?
2. **Quality check**: well-built? Style, patterns, no shortcuts?

Spec must pass before quality. Issues → subagent fixes → re-review.

---

## Context Discipline

- Subagents never inherit your session. Provide exactly what they need.
- Keeps their context small. Preserves yours for coordination.
- If a subagent asks questions → answer before they proceed.

---

## Rationalization Prevention

Excuses agents use to avoid delegation. None are valid.

| Excuse | Reality |
|--------|---------|
| "Too simple to delegate" | Simple tasks benefit from fresh context. Delegation takes 30 seconds. |
| "I already know how to do this" | Knowing how ≠ doing it right. Fresh eyes catch what you miss. |
| "Explaining to a subagent takes longer" | Focused prompt: 1 min. Your implementation with accumulated context: 5 min and more bugs. |
| "No specialist fits" | Use `general`. Every task can be delegated. |
| "I'll just fix this one quickly" | "Quick things" are how bugs enter the codebase. |
| "Setting up subagents is overhead" | It's quality assurance. The review alone justifies it. |
| "I'll delegate the next one" | Every "next one" is still "this one" to the future you. |

---

## Gotchas

- Without a focused prompt, subagents wander — every prompt needs goal, scope, constraints, output.
- Skipping review saves 2 minutes now and costs 20 later when bugs surface.
- Implementing "just this once" breaks delegation discipline for the entire session.
