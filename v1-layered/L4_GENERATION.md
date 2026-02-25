# Layer 4: Generation

> **Ownership: AI-Owned**

## Purpose

Produce all **derivable artifacts** from contracts. If it can be mechanically produced from the layers above, a human should not be writing it. This is the layer where the cost inversion of the AI era is most visible — what used to consume 80% of engineering effort becomes automated output.

---

## What Gets Generated

### 1. Implementation Code

AI generates the implementation from the contract. The prompt pattern:

```
Context loaded:
  - outcome-definition.md
  - glossary.md
  - appointment-schema.md
  - reminder-scheduling.contract.md

Prompt: "Implement the reminder-scheduling unit according to its
contract. Use TypeScript. Follow the project conventions in
glossary.md. Every rule in the contract must be implemented.
Every invariant must be enforced."
```

### 2. Tests

Generated from the contract's acceptance criteria, edge cases, and invariants — *without seeing the implementation*. This is the Cleanroom principle: tests derived from spec, not from code.

```
Test categories (all generated from contract):
├── Happy path tests (from Acceptance Criteria)
├── Edge case tests (from Edge Cases section)
├── Invariant tests (property-based, from Invariants)
├── Boundary tests (from Rules — at the thresholds)
└── Integration contract tests (from Inputs/Outputs schemas)
```

### 3. API Documentation

Generated from the contract's Inputs/Outputs and Rules sections. Not hand-written.

### 4. Traceability Updates

AI updates the traceability matrix:

```
Outcome: Reduce missed appointments <10%
  → Opportunity: Users forget appointments
    → Solution: SMS reminder 24hrs before
      → Contract: reminder-scheduling.contract.md (Rule 1)
        → Implementation: src/reminder-scheduling/index.ts:L42-L58
          → Test: tests/reminder-scheduling/24hr-reminder.test.ts
```

### 5. Change Impact Analysis

When a contract changes, AI identifies:
- Which implementations need regeneration
- Which tests are affected
- Which downstream contracts are impacted
- Whether the traceability chain is broken

---

## Regeneration vs. Editing

A key principle: **prefer regeneration over editing**. If a contract changes, don't patch the implementation — regenerate it from the updated contract. This prevents drift between intent and code.

When regeneration isn't practical (performance-optimized code, complex integrations), the AI generates a **diff recommendation** and a human reviews.

---

## Quality Gates (Automated)

Before any generated code exits this layer:

```
Gate 1: Does it compile/type-check?                    [automated]
Gate 2: Do all generated tests pass?                   [automated]
Gate 3: Do contract invariants hold under fuzzing?     [automated]
Gate 4: Does test coverage meet threshold?             [automated]
Gate 5: Security scan (SAST, dependency audit)         [automated]
Gate 6: Does it conform to project conventions?        [automated]
```

Only Gate 7 (alignment review) requires a human. That's Layer 5.

---

## Ownership Analysis

### Why This Layer Is AI-Owned

Layer 4 is pure transformation. Given a well-defined contract (Layer 3), generating code, tests, and documentation is a **derivation task**, not a judgment task. The inputs are formal enough that the outputs are deterministic-ish.

### What Would Make This Fail

| Risk | Mitigation |
|---|---|
| Contract is ambiguous | Layer 3 quality checklist catches this |
| Generated code is subtly wrong | Tests are generated independently from the same contract |
| Performance requirements not in contract | Add non-functional requirements to contract format |
| AI hallucinates capabilities | Automated gates catch compilation/test failures |
| Generated code drifts from contract over edits | Prefer regeneration over editing |

### The Honest Assessment

This is the most naturally AI-owned layer. The only human involvement is:
- **Spot-checking** generated code for subtle issues the gates miss
- **Performance tuning** when automated generation produces correct but slow code
- **Overriding** when the AI's implementation choice conflicts with an unwritten constraint

These are exceptions, not the norm.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Generate implementation | Spot-checks | Authors from contract |
| Generate tests | Spot-checks | Authors from contract (independently) |
| Generate documentation | Validates | Authors from contract + code |
| Maintain traceability | Audits periodically | Updates continuously |
| Run quality gates | Reviews failures | Executes all gates |
| Change impact analysis | Reviews recommendations | Identifies affected artifacts |
| Regeneration decisions | Approves when not automatic | Proposes regeneration vs. edit |
