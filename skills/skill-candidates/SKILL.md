---
name: skill-candidates
description: Use this skill during wiki ingest when you detect a procedural pattern that could apply to future tasks. Track from first encounter, propose skill creation at 3+ encounters. Activates on every ingest evaluation. Do NOT track declarative knowledge, one-off fixes, or patterns already covered by existing skills.
---

# Skill Candidates

Detect procedural patterns that repeat across tasks in the **current workspace** (where `AGENTS.md` lives). Propose skill creation when a pattern proves itself.

> **Scope:** `wiki/skill-candidates/` is local to the workspace. Skills promoted from candidates live in `.agents/skills/` within the workspace. Redundancy checks cover globally installed skills + workspace-local skills.

---

## <HARD-GATE> Auto-Activation During Ingest

When running wiki ingest, evaluate whether this task involved a procedural pattern worth tracking. Do NOT wait passively — actively assess after every task.

**Auto-activate when ALL are true:**

1. Task required a specific sequence of steps or non-obvious convention
2. Procedure could apply to future similar tasks
3. Pattern is procedural (how to do), not declarative (what something is)
4. No existing skill covers it

**Skip for:** one-off fixes, architectural decisions, facts about the codebase, standard tool operations with no domain-specific logic.

---

## Track vs Propose

Auto-activation always triggers evaluation. The question is what to DO:

- **1st or 2nd encounter** → create/update candidate file, increment encounters. Track it.
- **3rd or later encounter** → set `status: propose`, ask user if they want a skill.

Track immediately, propose at 3+. Do NOT skip tracking on first encounter.

---

## Tracking

One candidate per file in `wiki/skill-candidates/`.

**First encounter** — create `wiki/skill-candidates/<pattern-name>.md`:

```markdown
---
name: <pattern-name>
encounters: 1
status: candidate
---

## Trigger
[brief: what task context activates this]

## Why a skill
[what goes wrong or gets missed without it]
```

Add to `wiki/index.md` under `## Skill Candidates`:

```markdown
- [skill-candidates/<pattern-name>.md](skill-candidates/<pattern-name>.md) — [one-line description] (encounters: 1)
```

**Subsequent encounters** — read the file, increment `encounters`, append one line to `## Trigger` with task context.

When `encounters >= 3` and `status: candidate`, set `status: propose` and ask:

> "Pattern '<name>' has appeared in 3+ tasks: [brief description]. Create a skill for it?"

---

## Promotion

User approves → follow this sequence. Skipping cleanup creates stale candidates and broken index links.

1. **Check for redundancy.** Scan globally installed skills, currently loaded skills, and `.agents/skills/`. Similar skill exists?
   - Abort creation
   - Tell user: "Skill 'X' already covers this pattern. Absorbing into existing skill instead."
   - Update existing skill if candidate adds new context
   - Delete candidate file, remove from `wiki/index.md`
2. **Create the skill locally.** Follow `agents-skills/SKILL.md` spec. Create `.agents/skills/<name>/SKILL.md`. Directory name must match `name:` in frontmatter.
3. **Clean up.** Delete candidate file. Remove entry from `wiki/index.md`.

**User says no:** set `status: rejected`. Never propose again.

---

## Examples

- ✅ Third time forgetting `prisma migrate dev` after schema edit → propose skill
- ✅ Third task requiring the same 4-step release checklist → propose skill
- ✅ First time encountering a non-obvious multi-step deploy → track as candidate (encounters: 1)
- ❌ Learned the API uses JWT tokens → wiki, not candidate
- ❌ Found out project uses pnpm instead of npm → wiki, not candidate

---

## Rationalization Prevention

Excuses agents use to skip candidate evaluation. None are valid.

| Excuse | Reality |
|--------|---------|
| "Too specific to track" | Track anyway. Stays at encounters: 1 if it doesn't repeat. No harm. |
| "Only saw it once" | First encounter = track. You can't know if it repeats until you track. |
| "An existing skill probably covers this" | Check, don't assume. If yes → absorb. If no → track. |
| "I'll remember this pattern" | You won't. That's what tracking is for. |
| "Not worth creating a skill" | That's for the user to decide at encounters: 3. Track now. |

---

## Anti-patterns

- Candidates for declarative knowledge → belongs in wiki pages, not candidates.
- Waiting for 5+ encounters → 3 is enough.
- Tracking patterns specific to a single file or component.
- Creating a candidate when an existing skill covers it → absorb instead.
