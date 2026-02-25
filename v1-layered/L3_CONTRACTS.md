# Layer 3: Contracts

> **Ownership: AI-Authored, Human-Corrected**

## Purpose

Define **what each unit must do** without specifying **how**. Contracts are the bridge between human intent and machine generation. They are the single most important artifact in this model because they sit at the boundary between alignment (human) and execution (AI).

---

## What a Contract Contains

```markdown
# Contract: reminder-scheduling

## Purpose
Determine when and through which channel a reminder should be sent
for an upcoming appointment.

## Inputs
- Appointment (see shared/appointment-schema.md)
- UserPreferences: { channels: ["sms", "push", "email"], quietHours: TimeRange }
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
2. Second reminder: 2 hours before appointment (priority: "urgent")
3. No reminders during user's quiet hours — shift to nearest edge
4. If appointment is less than 3 hours away when created, send
   immediate reminder only
5. Channel selection: use first available in user's preference order

## Edge Cases
- Appointment cancelled after reminder scheduled → emit CancellationEvent
- User has no valid channels → log warning, no reminder sent
- Appointment time is in the past → no reminder, log error
- Quiet hours span the entire 24hr pre-appointment window → send at
  appointment creation time instead

## Invariants
- A reminder is never sent after the appointment time
- A reminder is never sent during quiet hours
- Every scheduled reminder maps to exactly one appointment
- No duplicate reminders for the same appointment + channel + time

## Dependencies
- shared/appointment-schema.md
- shared/glossary.md

## Acceptance Criteria
- Given an appointment 48hrs from now, two reminders are scheduled
- Given an appointment 1hr from now, one immediate reminder is sent
- Given quiet hours 10pm-8am and appointment at 9am, 24hr reminder
  shifts to 8am (not 9am the day before)
```

---

## Contract Quality Checklist

A contract is good enough when:

- [ ] An AI can generate an implementation without asking clarifying questions
- [ ] An AI can generate tests without seeing the implementation
- [ ] A new team member can understand what the unit does without reading code
- [ ] The contract fits in a context window with room for the implementation
- [ ] Every rule traces to a node on the opportunity tree

---

## AI's Role in Contracts

AI doesn't just assist — it **authors the first draft**. Humans correct and refine.

### Draft Generation
Given a well-defined Layer 1 (outcome) and Layer 2 (decomposition), AI generates the initial contract:

```
Prompt: "Given the outcome definition and the decomposition map,
draft a contract for the reminder-scheduling unit. Include purpose,
inputs, outputs, rules, edge cases, invariants, and acceptance criteria."
```

### Stress Testing
AI stress-tests its own contracts (or human-authored ones):

```
Prompt: "Here is a contract for reminder-scheduling.
Find gaps: edge cases I missed, ambiguous rules,
contradictions between rules, and invariants that
my rules might violate."
```

This is where the Cleanroom insight applies — **the spec review is more valuable than the code review**.

---

## Contract Versioning

Contracts evolve. When they change, the AI regenerates affected implementations and tests. The human reviews whether the contract *change* aligns with the outcome (Layer 5).

```
## Change Log
- v1.0: Initial contract
- v1.1: Added quiet hours edge case (shifted reminder behavior)
         Traced to: Opportunity — users silencing phones at night
```

---

## Ownership Analysis

### What's Actually Happening When a Human Does This

```
1. Translate business rules into preconditions/postconditions
2. Identify edge cases from domain knowledge
3. Set invariants based on what "must always be true"
4. Define the boundary between this unit and others
5. Decide what to handle vs. what to explicitly exclude
```

### What Would AI Need to Own This

| Sub-task | Barrier | Type |
|---|---|---|
| Translate business rules | If Layer 1 outcome + opportunity tree is detailed enough, rules are derivable | **Solvable** — if upstream layers are rich enough |
| Identify edge cases | AI is arguably *better* at exhaustive edge case enumeration than humans | **AI advantage** — humans miss edge cases, AI is systematic |
| Set invariants | Requires understanding what "must never break" — a business judgment | **Values question** — but derivable from business rules + risk appetite |
| Define boundaries | Determined by Layer 2 decomposition | **Already decided upstream** |
| Decide inclusions/exclusions | Requires understanding the contract's role in the larger system | **Solvable** — if decomposition is well-defined |

### The Honest Assessment

**This is the layer most ready to flip.** If Layer 1 (outcomes) and Layer 2 (decomposition) are well-defined, contracts are largely **derivable**. The business rules, edge cases, and invariants are implied by the outcome + the boundary definitions. AI can draft a contract that's 90% correct, and the human reviews the 10% that requires judgment about priorities and trade-offs.

The *real* question is: can a human reliably review an AI-generated contract for alignment? That's Layer 5's job, and it becomes critical if Layer 3 flips to AI-owned.

---

## Irreducible Human Input

```
IRREDUCIBLE:
├── Priority calls: "If rules conflict, reliability beats performance"
├── Domain secrets: Business rules that aren't written anywhere
├── Review & approval of the generated contract
└── "That's not what I meant" corrections

AI-GENERATABLE:
├── Contract drafting from outcome + decomposition
├── Edge case enumeration (AI is better at this)
├── Invariant proposals from schema analysis
├── Acceptance criteria generation
├── Consistency checks across contracts
└── Gap analysis ("your contract doesn't cover X")
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Draft the contract | Reviews & corrects | Authors first draft from upstream layers |
| Identify edge cases | Validates business relevance | Enumerates systematically |
| Set invariants | Confirms "must never break" | Proposes from schema + rules |
| Write acceptance criteria | Validates against intent | Generates from rules + edge cases |
| Version management | Approves changes | Tracks versions, flags drift |
| Cross-contract consistency | Reviews flags | Checks for contradictions across units |
