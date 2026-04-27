---
name: parallel-work
description: Use this skill when 2 or more tasks touch different files or subsystems with no shared state and no sequential dependencies. Always split independent work across parallel specialist agents — one per domain. Parallel execution reduces context per agent, optimizing token consumption and output quality. Do NOT use when tasks share files or one affects another's outcome.
---

# Parallel Work

Run independent tasks concurrently. One subagent per domain — no overlap, no shared state. Parallel = N tasks in the time of 1. Each agent gets smaller context → better results with fewer tokens.

---

## <HARD-GATE> Parallelization Mandate

2+ independent tasks (no shared files, no dependencies) → dispatch in parallel. Sequential execution of independent tasks is a violation.

**Independence check:**
- [ ] Tasks touch different files/subsystems
- [ ] No task depends on another's output
- [ ] No shared mutable state

All checks pass → dispatch in parallel. Any fail → sequential or decompose further.

If you think you "need full system context," decompose the task — there's almost always a way to isolate domains.

---

## Procedure

1. **Group by independence.** Tasks touching different code with no overlap = separate domains.
2. **One prompt per domain.** Focused, self-contained:
   - Goal: what to accomplish
   - Scope: which files/subsystem
   - Context: everything needed — no session references
   - Constraints: what not to change
   - Output: what to return
3. **Launch all at once.** Each subagent runs isolated.
4. **Integrate on return.** Read summaries, check for conflicts, run tests.

---

## Prompt Quality

Vague prompts → lost subagents. Be specific.

**Weak:** "Fix the auth tests"
**Sharp:** "Fix 2 failing tests in `src/auth/login.test.ts`: 'should reject expired token' and 'should refresh token'. Root cause likely in `src/auth/token.ts`. Don't modify other files. Return: what you found and fixed."

---

## After Integration

Run full test suite. Independent fixes can create unexpected interactions.

---

## Conflict Handling

Two subagents edited the same file → manual merge. Shouldn't happen if domains were properly separated. If it does, reassess independence for next time.

---

## Rationalization Prevention

Excuses agents use to skip parallelization. None are valid.

| Excuse | Reality |
|--------|---------|
| "Sequential is simpler to manage" | Simpler ≠ better. Parallel is faster and gives each agent smaller context. |
| "Only 2 tasks, not worth it" | 2 in parallel = half the time. Always worth it if independent. |
| "I'll lose track of parallel agents" | You review each result as it returns. Same as sequential, just faster. |
| "Parallel might cause conflicts" | Only if you misidentify independence. Run the check. |
| "Tasks are small enough to combine" | Combining increases agent context. Keep them separate. |

---

## Gotchas

- Launching parallel agents that share files guarantees conflicts — verify file independence before dispatching.
- Without specific prompts, each subagent re-explores the codebase and wastes time on context you already have.
- Sequential execution of independent tasks is not "safer" — it's slower and inflates each agent's context.
