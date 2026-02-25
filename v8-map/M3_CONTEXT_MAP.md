# Context Map

> **"How do domains relate — and where does translation happen?"**

## Purpose

From Domain-Driven Design. Shows the relationships between bounded contexts (domains) and identifies where **translation, shared models, and anti-corruption layers** are needed. Boundary relationships are where alignment is most fragile.

## Relationship Types

```
SHARED KERNEL:
  Two contexts share a model directly.
  Change requires coordination.
  Alignment cost: HIGH
  Context window impact: Both units must load the shared model.

CUSTOMER-SUPPLIER:
  Upstream produces, downstream consumes.
  Consumer defines what it needs (consumer-driven contracts).
  Alignment cost: MEDIUM
  Context window impact: Load interface contract only, not internals.

CONFORMIST:
  We adopt an external model as-is (no translation).
  Alignment cost: LOW (we conform)
  Risk: Tightly coupled to external changes.
  Context window impact: Load external API docs.

ANTI-CORRUPTION LAYER:
  Translation layer between our model and an external/legacy system.
  Alignment cost: MEDIUM (translation logic)
  Context window impact: Load both models + translation rules.

SEPARATE WAYS:
  No relationship. Contexts are fully independent.
  Alignment cost: NONE
  Context window impact: Never load together.
```

## How to Use for AI Context Loading

The context map tells you **what to load together** and **what to keep separate**:

```
IF relationship is SHARED KERNEL:
  → Load both contexts' specs when working on either
  → Changes require cross-context clarity test

IF relationship is CUSTOMER-SUPPLIER:
  → Load only the interface contract, not the other context's internals
  → Consumer drives the contract definition

IF relationship is SEPARATE WAYS:
  → Never load both. No shared context needed.
  → Maximum context budget available for each independently.
```

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Identify bounded contexts | Decides domain boundaries | Suggests from code coupling analysis |
| Classify relationships | Decides relationship type | Proposes from import/dependency analysis |
| Design anti-corruption layers | Architects the translation | Generates translation code from both models |
| Detect relationship drift | Reviews when issues arise | Monitors for coupling that crosses context boundaries |
