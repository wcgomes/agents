---
name: wiki
description: Use this skill BEFORE any codebase exploration or task execution — query wiki/index.md first to leverage existing knowledge; never explore blindly. Also use at the end of every task to ingest what was learned. Self-learning loop — never skip ingest. Activates on every task.
---

# Wiki

Workspace knowledge base. Lives in `wiki/` in the **current working directory** (where `AGENTS.md` was copied). Self-learning loop — every task leaves the workspace better documented.

> **Scope:** operates on the workspace being worked on, not on the global skills directory.

---

## Structure

One concept per page. Focused pages over broad ones. Subdirectories when a topic grows. Link related concepts.

If `wiki/` doesn't exist, create it immediately with `wiki/index.md`.

```
wiki/
├── index.md              # Index of all pages — always up to date
├── architecture.md       # How the system is structured
├── conventions.md        # Code patterns, naming, style
├── domain.md             # Business rules and domain logic
├── decisions.md          # ADRs — why X over Y
├── skill-candidates/     # Recurring patterns tracked for skill promotion
└── ...
```

---

## index.md

Mandatory. Update when pages added or removed. Each entry needs a brief description — pages identifiable without opening:

```markdown
## Pages

- [architecture.md](architecture.md) — how the system is structured
- [auth/overview.md](auth/overview.md) — authentication flow and token lifecycle

## Skill Candidates

- [skill-candidates/prisma-migrate.md](skill-candidates/prisma-migrate.md) — run migrate + generate after schema changes (encounters: 1)
```

---

## Three Operations

**Query** — start by reading `wiki/index.md`. Use descriptions to identify relevant pages — load only those. Never answer from memory if a wiki page exists.

**Ingest** — end of every task, evaluate automatically (checklist below). Update `index.md`, link from related pages.

**Lint** — after task, scan for contradictions, orphan pages not in `index.md`, stale claims. Fix what you find.

---

## <HARD-GATE> Auto-Evaluation at End of Task

Run this checklist at the end of EVERY task. Do NOT skip. Do NOT decide "nothing to ingest" without evaluating.

1. Architectural decision made? → `decisions.md`
2. New code pattern or convention? → `conventions.md`
3. Domain rule clarified or corrected? → `domain.md`
4. System structure insight? → `architecture.md`
5. User corrected a misunderstanding? → ingest where relevant
6. Non-obvious multi-step procedure? → load `skill-candidates` skill

Any YES → ingest. All NO → skip (but you must have evaluated each).

Do NOT ask the user "should I update the wiki?" — evaluate automatically. Only ask if the scope of the update is ambiguous.

---

## <HARD-GATE> Auto Skill-Candidate Evaluation

During ingest, if you performed a non-obvious sequence of steps that could apply to future tasks, load the `skill-candidates` skill.

**Auto-activate when:**
- Multi-step procedure (3+ steps)
- Required domain knowledge or convention (not obvious)
- Could apply to future similar tasks

**Skip when:**
- One-line change with no domain knowledge
- Already covered by an existing skill
- Standard tool operations with no domain-specific logic

---

## Rationalization Prevention

Excuses agents use to skip wiki operations. None are valid.

| Excuse | Reality |
|--------|---------|
| "I didn't learn anything new" | Run the checklist. You may have learned something you didn't notice. |
| "This is too minor to document" | Minor insights compound. If it clarified something, ingest it. |
| "The wiki is already up to date" | Run the checklist to verify. Don't assume. |
| "I'll update it later" | You won't. Do it now while context is fresh. |
| "No need to ask the user, I'll just skip it" | You don't ask. You evaluate automatically. |

---

## Gotchas

- Always read `wiki/index.md` first — loading all pages wastes context.
- After `index.md` points you to a page, use `grep` to find the section, then `read` with `offset`/`limit`. Never pull a large `.md` whole.
- Pages not linked from `index.md` are invisible — always update the index.
- Asking "should I update the wiki?" is a failure — evaluate automatically.
