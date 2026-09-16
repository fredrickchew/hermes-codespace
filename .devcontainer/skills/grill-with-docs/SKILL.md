---
name: grill-with-docs
description: Sharpen a plan/design via interview while writing domain glossary and ADRs as we go. Use when planning, designing, or recording codebase decisions.
---

# Grill with Docs

A relentless interview to sharpen a plan or design, which also creates docs —
ADRs and a glossary — as we go. This merges two behaviors (grilling + domain
modeling) into one active discipline. Reading `CONTEXT.md` for vocabulary is
not this skill; this is for when you're *changing* the model, not just
consuming it.

## When to use

- The user is sketching out a design, architecture, or feature plan and wants it stress-tested.
- The user is discussing codebase terminology or deciding on a name for a concept.
- The user wants to record a decision (ADR) or build/update the domain glossary (`CONTEXT.md`).

## File structure

Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts and it
maps where each lives (system-wide ADRs at root `docs/adr/`, context-specific
ones under each context's `docs/adr/`). See
`references/CONTEXT-FORMAT.md`.

Create files lazily: only when you have something to write. If no `CONTEXT.md`
exists, create one when the first term is resolved. If no `docs/adr/` exists,
create it when the first ADR is needed.

## During the session

### Interview mode (the grilling)

Relentlessly probe the plan or design before committing to it. Ask the
questions worth asking — edge cases, contradictions, cost of changing your
mind, who owns what. Do not let vague or overloaded terms pass.

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in
`CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as
X, but you seem to mean Y. Which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term.
"You're saying 'account': do you mean the Customer or the User? Those are
different things."

### Discuss concrete scenarios

When domain relationships are discussed, stress-test them with specific
scenarios. Invent scenarios that probe edge cases and force the user to be
precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you
find a contradiction, surface it: "Your code cancels entire Orders, but you
just said partial cancellation is possible. Which is right?"

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` right there. Don't batch these up:
capture them as they happen. Follow `references/CONTEXT-FORMAT.md`.

`CONTEXT.md` must stay totally devoid of implementation details. It is a
glossary and nothing else — not a spec, not a scratch pad, not a repo for
implementation decisions.

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse**: the cost of changing your mind later is meaningful.
2. **Surprising without context**: a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off**: there were genuine alternatives and you picked one for specific reasons.

If any of the three is missing, skip the ADR. Follow `references/ADR-FORMAT.md`.

## References

- `references/ADR-FORMAT.md` — ADR template and timing rules.
- `references/CONTEXT-FORMAT.md` — domain-glossary (`CONTEXT.md`) format and rules.