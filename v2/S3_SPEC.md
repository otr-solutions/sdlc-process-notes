# Station 3: SPEC

> **"What exactly does this unit do?"**

## Role in the Flow

SPEC receives shaped units (with clear boundaries and interfaces) and defines **precisely what each unit does** — preconditions, postconditions, invariants, edge cases. This is the Platform-Independent Model from Model-Driven Architecture: it says WHAT without committing to HOW.

### Pull Relationships
- SPEC pulls from SHAPE: "I need a unit with defined boundaries to specify."
- MAKE pulls from SPEC: "I need a validated spec to generate from."

SPEC is the **critical handoff point** — the last station where human judgment is routinely needed before AI takes over in MAKE.

---

## WIP Limit: 5-8

Specs should stay slightly ahead of MAKE — a small buffer of validated specs ready for generation. Too many unbuilt specs = wasted alignment effort (specs drift). Too few = MAKE starves.

The buffer size depends on AI generation speed. If MAKE can build a unit in 10 minutes, you need very few specs buffered. If integration testing takes hours, buffer more.

---

## V-Model Pairing: Contract Validation

```
SPEC ◄──────────────────────────────────► CONTRACT VALIDATION

"These rules, these invariants,          "Generated code satisfies
 these edge cases"                        all rules and invariants?"
```

Contract Validation is **AI-owned** — it's formal verification of generated code against the spec. This is where the V-Model's symmetry pays off: the spec and its validation are defined simultaneously, ensuring nothing falls through.

---

## Model-Driven Architecture Mapping

MDA defines three abstraction levels. SPEC is the middle one:

```
MDA Level                         V2 Station
─────────                         ──────────
CIM (Computation-Independent)  →  INTENT (business outcome, no tech)
PIM (Platform-Independent)     →  SPEC (what the unit does, no how)
PSM (Platform-Specific)        →  MAKE (TypeScript + PostgreSQL + AWS)
```

The key MDA insight: **the PIM (spec) is the stable artifact**. Platforms change (TypeScript today, something else tomorrow). Business outcomes change slowly. But the spec — what the unit does — changes at a predictable, moderate rate.

### Transformation Rules

MDA defines transformations between levels. In V2:

```
INTENT → SPEC:  AI drafts spec from outcome + unit boundaries
                 (CIM → PIM transformation)

SPEC → MAKE:    AI generates code + tests from spec
                 (PIM → PSM transformation)
```

Both transformations are AI-driven. The human role is **validating the transformations**, not performing them.

---

## PDCA Micro-Cycle

```
PLAN:  AI drafts spec from intent + unit card
       Loads: outcome card, glossary, schemas, unit card

DO:    Human reviews the draft
       Adds domain knowledge AI doesn't have
       Corrects priority calls and trade-offs

CHECK: Clarity test — feed spec back to AI:
       "Generate an implementation from this spec."
       Count clarifying questions.
       Zero questions = spec is ready.

ACT:   Refine ambiguous sections based on AI's questions.
       Repeat until clarity test passes.
```

### The Clarity Test

This is V2's unique contribution to spec quality. Instead of a subjective checklist, specs have an **objective, repeatable quality test**:

```
1. Give the spec to AI without any other context
2. Ask AI to implement it
3. Count clarifying questions AI asks

Score:
  0 questions = READY (spec is unambiguous)
  1-2 questions = ALMOST (minor gaps)
  3+ questions = NOT READY (significant ambiguity)
```

This test is itself AI-executed — making spec quality a measurable, automatable metric.

---

## Artifacts

### The Spec Document

```markdown
# Spec: reminder-scheduling

## Purpose
Determine when and through which channel a reminder should be sent
for an upcoming appointment.

## Inputs
- Appointment (see shared/appointment-schema)
- UserPreferences: { channels: ["sms","push","email"], quietHours: TimeRange }
- CurrentTime: ISO-8601 timestamp

## Outputs
- ReminderEvent: {
    appointmentId: string,
    userId: string,
    channel: "sms" | "push" | "email",
    scheduledFor: ISO-8601 timestamp,
    message: string,
    priority: "standard" | "urgent"
  }

## Rules
1. First reminder: 24 hours before appointment
2. Second reminder: 2 hours before (priority: "urgent")
3. No reminders during quiet hours — shift to nearest edge
4. Appointment < 3 hours away → immediate reminder only
5. Channel: first available in user's preference order

## Edge Cases
- Cancelled after scheduled → emit CancellationEvent
- No valid channels → log warning, skip
- Past appointment → log error, skip
- Quiet hours span entire window → send at creation time

## Invariants
- Never sent after appointment time
- Never sent during quiet hours
- One reminder per appointment + channel + time
- Every reminder maps to exactly one appointment

## Clarity Score: 0 questions (READY)
## Version: 1.1
## Change Log:
  - v1.0: Initial spec
  - v1.1: Added quiet hours edge case
```

---

## Ownership

### Theory of Constraints View

SPEC is where the constraint (human alignment judgment) meets the non-constraint (AI generation). It's the **handoff point**. Optimizing this handoff is critical:

- **Fast spec validation** = MAKE doesn't starve
- **High spec quality** = MAKE doesn't produce garbage
- **Small spec size** = fits in context, leaves room for generation

The ideal: humans spend minimal time per spec (because AI drafts well) but that time is **maximally impactful** (catching the gaps AI misses).

### Who Does What

| Activity | Human | AI |
|---|---|---|
| Draft the spec | Reviews & corrects | Authors first draft from intent + shape |
| Enumerate edge cases | Validates business relevance | Enumerates systematically |
| Set invariants | Confirms "must never break" | Proposes from schema + rules |
| Write acceptance criteria | Validates against intent | Generates from rules + edges |
| Run clarity test | Reviews AI's questions | Executes test, scores |
| Version management | Approves changes | Tracks versions, flags drift |
| Cross-spec consistency | Reviews flags | Checks for contradictions |
| Contract validation (V-pair) | Spot-checks | Verifies code against spec |

### Irreducible Human Input

```
├── Priority calls: "If rules conflict, reliability > performance"
├── Domain secrets: Business rules that aren't written anywhere
├── "That's not what I meant" corrections
└── Approval that spec is ready for MAKE
```
