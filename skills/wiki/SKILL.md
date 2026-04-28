---
name: wiki
description: Use this skill when querying workspace knowledge before tasks or ingesting learnings after completing work. Self-learning loop for the workspace.
---

# Wiki

Workspace knowledge base and self-improvement loop. Query wiki before exploring blindly. Ingest learnings after every task.

---

## Two Operations

**Setup** — create `wiki/` immediately if it doesn't exist.

**Query** — read `wiki/index.md` first. Identify relevant pages. Load only those.

**Ingest** — end of every task, evaluate automatically if anything was learned.

---

## <HARD-GATE> Auto-Evaluation at End of Task

Run this checklist at the end of EVERY task. Do NOT skip.

1. Architectural decision made? → `wiki/decisions/<decision-name>.md`
2. New code pattern or convention? → `wiki/conventions/<pattern-name>.md`
3. Domain rule clarified or corrected? → `wiki/domain/<rule-name>.md`
4. System structure insight? → `wiki/architecture.md`
5. User corrected a misunderstanding? → ingest where relevant
6. Non-obvious multi-step procedure? → evaluate for skill candidate

Any YES → ingest. All NO → skip (but you must have evaluated each).

Do NOT ask "should I update the wiki?" — evaluate automatically.

---

## Wiki Structure

```
wiki/
├── index.md              # Always up to date — update when pages added/removed
├── architecture.md       # System structure overview (single file)
├── conventions/          # One file per convention
│   └── <pattern-name>.md
├── domain/               # One file per business rule
│   └── <rule-name>.md
├── decisions/            # One file per ADR
│   └── <decision-name>.md
├── skill-candidates/     # Recurring patterns tracked for skill promotion
│   └── <pattern-name>.md
└── ...
```

Create `wiki/` immediately if it doesn't exist (see Wiki Structure below).

---

## <HARD-GATE> Auto Skill-Candidate Evaluation

If you performed a non-obvious sequence of steps that could apply to future tasks, evaluate for skill candidate.

**Activate when ALL:**
- Multi-step procedure (3+ steps)
- Required domain knowledge or convention (not obvious)
- Could apply to future similar tasks

**Skip when:**
- One-line change with no domain knowledge
- Already covered by existing skill
- Standard tool operations with no domain-specific logic

---

## Skill Candidate Tracking

One candidate per file in `wiki/skill-candidates/`.

### Track (1st or 2nd encounter)

Create `wiki/skill-candidates/<pattern-name>.md`:

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

Add to `wiki/index.md` under `## Skill Candidates`.

### Propose (3rd+ encounter)

When `encounters >= 3` and `status: candidate`, set `status: propose` and ask user.

### Promotion Sequence

1. Check for redundancy (globally installed skills + workspace skills)
2. Create skill locally at `.agents/skills/<name>/SKILL.md`
3. Delete candidate file, remove from `wiki/index.md`

---

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "I didn't learn anything new" | Run the checklist. |
| "This is too minor to document" | Minor insights compound. |
| "I'll update it later" | You won't. |
| "Too specific to track" | Track anyway. |

---

## Gotchas

- Always read `wiki/index.md` first — loading all pages wastes context.
- Pages not linked from `index.md` are invisible.
- Evaluate automatically — don't wait for the user to ask.
