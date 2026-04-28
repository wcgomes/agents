# AGENTS.md

## System Goal

Enforce a `subagent-driven workflow`.

Default behavior:
- discover specialists before execution
- select the best semantic match
- delegate when an eligible specialist exists
- use a generalist only as explicit fallback

---

## Core Principles

- `specialist-first` is mandatory
- generalist is fallback, not peer
- discovery and selection happen before execution
- capability alone is never sufficient when a better specialist exists
- delegation distributes work, not accountability
- fallback must be brief, explicit, and defensible
- subdelegation is allowed when it improves specialization or parallelism

---

## Discovery Requirement

Before specialist work begins, inspect agent sources exposed by the current environment.

Discovery policy:
- consider supported sources in the current platform context
- include workspace-scoped and user/global sources when exposed
- assess candidates from available identifier, description, and declared specialization when exposed
- ignore invalid, incomplete, or incompatible agents
- produce an eligible candidate set before execution
- if discovery is incomplete, use the best supported path and state that limitation before fallback

Do not bind this policy to one platform's commands, paths, or installation layout.

---

## Selection Rules

Choose specialists by semantic fit, not convenience.

Selection policy:
- match by description, specialization, scope, and constraints
- prefer the most semantically specific eligible specialist
- allow multiple specialists when the task decomposes into independent scopes
- use stable documented tie-break rules when candidates are similarly suitable
- do not use a generalist if an eligible specialist exists
- do not bypass a specialist because the current agent could also do the work

Eligibility minimum:
- explicit domain or task match in the description
- no conflict with stated constraints
- scope compatible with the immediate task
- no clearly better eligible specialist

Tie-break rules:
- first: higher semantic specificity
- second: stable documented precedence in the current environment
- third: split across specialists if scopes are naturally distinct

---

## Delegation Requirement

Delegation is mandatory when an eligible specialist exists.

Required behavior:
- if a specialist is eligible, delegate
- if multiple specialists fit independent scopes, parallel delegation is allowed
- do not skip delegation because the current agent is "good enough"
- do not execute specialist work before discovery and selection are complete
- do not treat broad competence as an exception

---

## Generalist Fallback

Generalist execution is an exception path.

Fallback is allowed only when:
- no eligible specialist exists
- all discovered candidates are unsuitable for stated constraints
- delegation is explicitly constrained by the user
- delegation failed and fallback is allowed by the current task constraints

Fallback requirements:
- explain briefly why delegation did not apply
- keep the explanation factual and specific
- identify the failed discovery, selection, or constraint condition
- do not treat "I can do it" as sufficient justification
- do not silently fall back

Fallback mode:
- prefer delegating to a generalist agent when the environment supports it
- direct execution by the current agent is allowed only when delegation is constrained, unavailable, or already failed under the current task constraints

---

## Subdelegation

Subdelegation is allowed by policy.

Subdelegation rules:
- use it when specialization, parallelism, or decomposition improves the task
- the delegating agent remains responsible for review, synthesis, and quality
- do not use recursive ping-pong delegation
- avoid uncontrolled fan-out
- keep scopes explicit and non-overlapping unless integration is intentional
- do not subdelegate to avoid owning a hard decision or review step

Delegation does not transfer ownership upward.

---

## Execution Accountability

The agent that accepts a task owns the result it returns.

Accountability rules:
- review delegated output before integrating it
- verify conformance first, then quality
- do not pass delegated output upward without review
- when multiple agents contribute, synthesize before returning
- explain blockers instead of hiding them behind further delegation

---

## Invocation Contract

Every delegation should include enough context to operate without shared session assumptions.

Minimum content:
- task
- objective
- scope
- constraints
- relevant files or context
- expected deliverable
- expected return format

Do not rely on hidden context.
Do not ask delegated agents to infer missing task structure from prior conversation.
Provide enough explicit context to avoid avoidable clarification loops.

---

## Execution Flow

### Phase 1: Assess

1. Load `wiki`.
2. Read `wiki/index.md` before broad exploration when it exists.
3. Clarify done criteria when unclear.
4. Load `delegate` before deciding discovery, selection, or delegation.
5. Discover eligible specialists.
6. Select the best specialist or specialists.

Hard-gate before execution:
- [ ] done criteria defined or clarified
- [ ] discovery completed
- [ ] eligible candidates assessed
- [ ] delegation decision made under this policy
- [ ] any fallback explicitly justified

### Phase 2: Execute

1. Delegate specialist work using a structured handoff.
2. Parallelize only when scopes are non-overlapping and integration dependencies are absent or explicitly planned.
3. Allow subdelegation when it improves specialization or decomposition.
4. Review and synthesize before returning results upward.

### Phase 3: After Task

1. Load `wiki`.
2. Evaluate whether new knowledge should be ingested.
3. Review output for brevity and compliance with these rules.

---

## If Stuck

Two cycles with no progress:
- stop
- explain the blocker
- ask for direction if needed

Repeated delegated `NEEDS_CONTEXT` or `BLOCKED` without material progress counts as no progress.

---

## Universal Rules

### Brevity

Every word must carry information. Cut the rest.

- drop filler, pleasantries, and hedging
- use short words when they preserve meaning
- keep syntax compact
- preserve code, commands, paths, URLs, and technical terms exactly
- use full sentences for security warnings and irreversible actions

### Skill Activation

When a skill description fits, invoke it before anything else. No exceptions.

| Skill | Activates |
|---|---|
| `delegate` | Delegation, specialist discovery, selection, and review |
| `wiki` | Querying workspace knowledge before or after tasks |
| `implement` | Writing, editing, refactoring, fixing code |
| `debug` | Investigating errors, unexpected behavior |
| `agents-skills` | Creating, refining, validating Agent Skills |

## Instruction Priority

1. **User instructions** — highest priority
2. **Active skills** — mandatory when invoked, override defaults
3. **This file** — baseline protocol and rules
