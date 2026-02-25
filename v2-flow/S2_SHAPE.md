# Station 2: SHAPE

> **"What are the pieces, and where do the boundaries go?"**

## Role in the Flow

SHAPE receives validated intents and decomposes them into **context-window-sized units** with clear interfaces. It's the station where a big ambiguous outcome becomes small tractable pieces.

### Pull Relationships
- SHAPE pulls from INTENT: "I need an outcome to decompose."
- SPEC pulls from SHAPE: "I need a unit to specify."

SHAPE is the **translation layer** between business outcomes and technical units.

---

## WIP Limit: 3-5

Each active outcome (from INTENT) typically decomposes into 3-5 units. More than 5 active shapes and teams lose coherence — they can't hold the full picture of how units relate.

---

## V-Model Pairing: Integration Validation

```
SHAPE ◄──────────────────────────────────► INTEGRATION VALIDATION

"3 units, these boundaries,                "When units connect, does the combined
 these interfaces"                          behavior match what a user expects?"
```

Integration Validation is triggered when **two or more units from the same shape connect for the first time**. It answers: "Does the composition work, or do the seams leak?"

---

## Information Hiding (Parnas)

This is where Parnas's principle is most directly applied. Each unit IS an information-hiding module. The boundary decision is fundamentally a decision about **what to hide**.

```
UNIT: reminder-scheduling
  HIDES:                           EXPOSES:
  ─────                            ───────
  Scheduling algorithm             ReminderEvent (output contract)
  Quiet hours logic                Appointment (input contract)
  Channel selection strategy       UserPreferences (input contract)
  Internal state management        Error types
  AI generation approach           
```

### The Context Window as Information Hiding Boundary

Parnas argued modules should be defined by "likely change." In V2, we add a second criterion: **context window fit**. A unit should hide:

1. **What's likely to change** (Parnas's original criterion)
2. **What doesn't fit** (context window constraint)

```
Good boundary: Hides volatile internals AND fits in context
Bad boundary:  Exposes internals OR exceeds context budget
```

These two criteria usually align naturally — things that change together tend to be coupled, and coupled things should be in the same context window.

---

## Feature-Driven Development Mapping

FDD's feature template maps directly to V2 units:

```
FDD: <action> <result> <object>

"Schedule reminder for appointment"     → reminder-scheduling
"Deliver notification to user"          → notification-delivery
"Reschedule appointment from reminder"  → reschedule-flow
```

Each feature/unit:
- Delivers **recognizable value** (a user can see it work)
- Has a **clear owner** (one team, one AI session)
- Is **buildable independently** (with mocked interfaces)
- Fits in a **single context window** (with room to reason)

---

## PDCA Micro-Cycle

```
PLAN:  AI analyzes the intent and proposes unit boundaries
       Based on: coupling analysis, domain model, context budgets

DO:    Map dependencies, estimate context budgets, draft interfaces
       Apply Parnas: what does each unit hide?

CHECK: Does each unit fit in a context window?
       Is each unit cohesive (one reason to change)?
       Can each unit be specified independently?
       Does the set of units cover the entire intent?

ACT:   Split oversized units, merge over-fragmented ones.
       Repeat until all units pass the checks.
```

### The Context Window Test (Concrete)

```
For each unit, calculate:

  Shared context (outcome + glossary + schemas):  ~10K tokens
  Unit's spec:                                    ~5-10K tokens
  Unit's implementation:                          ~15-25K tokens
  ─────────────────────────────────────────────────────────
  Subtotal:                                       ~30-45K tokens
  AI reasoning headroom (40%):                    ~20-30K tokens
  ─────────────────────────────────────────────────────────
  TOTAL must be ≤ context window:                 ~60-75K tokens ✓

  If TOTAL > context window → unit must be split
  If unit's spec < 2K tokens → unit may be too small (over-fragmented)
```

---

## Artifacts

### Unit Card

One card per unit. Serves as the interface definition that other units depend on.

```
UNIT:        reminder-scheduling
INTENT:      Reduce missed appointments (Outcome A)
FEATURE:     "Schedule reminder for appointment"

HIDES:
  - Scheduling algorithm
  - Quiet hours resolution logic
  - Channel selection strategy

INTERFACE:
  INPUTS:    Appointment, UserPreferences, CurrentTime
  OUTPUTS:   ReminderEvent
  ERRORS:    InvalidAppointment, NoValidChannels

DEPENDENCIES:
  - shared/appointment-schema
  - shared/glossary

CONTEXT BUDGET:
  - Shared overhead: 10K tokens
  - Spec: ~8K tokens
  - Implementation: ~20K tokens
  - Headroom: ~25K tokens (40%)
  - Total: ~63K tokens ✓ fits

CHANGE PREDICTION:
  - High: business rules for when to send
  - Low: event shape, error types
```

### Dependency Map

```
shared/appointment-schema
    ├──→ reminder-scheduling
    │        └──→ notification-delivery
    └──→ reschedule-flow

Build order: shared → reminder-scheduling ──→ notification-delivery
                    └──→ reschedule-flow (parallel)
```

---

## Ownership

### Theory of Constraints View

SHAPE is the **second most human-intensive station**. Boundary decisions are strategic bets — which boundaries will hold over time? But much of the analytical work is AI-native:

- Coupling analysis: AI
- Cohesion metrics: AI
- Context budget calculation: AI
- Dependency mapping: AI
- **Where to actually cut**: Human judgment with AI proposals

### Who Does What

| Activity | Human | AI |
|---|---|---|
| Propose boundaries | Approves & constrains | Proposes from coupling analysis |
| Calculate context budgets | Validates | Counts tokens, flags oversize |
| Apply Parnas (what to hide) | Decides strategic hiding | Suggests based on change frequency |
| Map dependencies | Validates | Derives from contracts/imports |
| Predict rate of change | Provides strategic context | Analyzes git/ticket history |
| Integration validation (V-pair) | Judges emergent behavior | Tests interface compatibility |

### Irreducible Human Input

```
├── Organizational constraints: "Team A owns this, Team B owns that"
├── Sacred cows: "Don't modify the legacy service"
├── Strategic bets: "We think pricing changes often, delivery doesn't"
└── Approval of final decomposition
```
