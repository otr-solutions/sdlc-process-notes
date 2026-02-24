# Layer 2: Decomposition

> **Ownership: AI-Proposed, Human-Constrained**

## Purpose

Break the system into **units that each fit in a single AI context window** with clear boundaries. This is the most consequential architectural decision because it determines what AI can and cannot reason about in one pass.

---

## The Decomposition Rule

Each unit must satisfy:

```
Size:        All relevant code + contracts + context ≤ ~60% of context window
             (leave 40% for AI reasoning and generation)

Coherence:   The unit does ONE thing that maps to ONE branch of the
             opportunity tree

Independence: The unit can be built, tested, and validated without
              loading other units into context

Interface:   All communication with other units happens through
             explicitly defined contracts (not shared state, not
             implicit knowledge)
```

---

## How to Decompose

**Step 1: Start from the Opportunity Tree (Layer 1)**
Each solution branch is a candidate unit.

**Step 2: Apply the Context Window Test**
Can you describe this unit's full behavior — inputs, outputs, rules, edge cases, dependencies — in under ~50K tokens? If not, split it.

**Step 3: Identify Shared Concepts**
What vocabulary, data structures, or business rules appear in multiple units? These go into the `shared/` context — a glossary and set of shared contracts that get loaded into every AI session.

**Step 4: Map Dependencies**
Which units need to exist before others can be built? This becomes your build order.

---

## Artifacts

### System Decomposition Map

```
┌─────────────────────────────────────────────────────┐
│                  APPOINTMENT REMINDERS               │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Unit: reminder-scheduling                          │
│  ├── Owns: When and how reminders are triggered     │
│  ├── Depends on: shared/appointment-schema          │
│  ├── Produces: ReminderEvent                        │
│  └── Context budget: ~15K tokens                    │
│                                                     │
│  Unit: notification-delivery                        │
│  ├── Owns: SMS, push, email delivery                │
│  ├── Depends on: ReminderEvent contract             │
│  ├── Produces: DeliveryReceipt                      │
│  └── Context budget: ~20K tokens                    │
│                                                     │
│  Unit: reschedule-flow                              │
│  ├── Owns: One-tap reschedule UX + logic            │
│  ├── Depends on: shared/appointment-schema          │
│  ├── Produces: RescheduleConfirmation               │
│  └── Context budget: ~25K tokens                    │
│                                                     │
│  Shared Context (loaded into every AI session):     │
│  ├── glossary.md (~2K tokens)                       │
│  ├── appointment-schema.md (~3K tokens)             │
│  ├── outcome-definition.md (~4K tokens)             │
│  └── Total shared overhead: ~9K tokens              │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Dependency Graph

```
shared/appointment-schema
    ├──→ reminder-scheduling
    │        └──→ notification-delivery
    └──→ reschedule-flow
```

Build order: shared → reminder-scheduling → notification-delivery (can parallel with reschedule-flow)

### Boundary Decision Records

Why you drew the lines where you did. This is an ADR specifically for decomposition.

```
## BDR-001: Separate reminder-scheduling from notification-delivery

### Context
Reminders involve business logic (when, how often, which channel).
Delivery involves infrastructure (SMS providers, push tokens, retry logic).

### Decision
Separate them. Scheduling produces a ReminderEvent. Delivery consumes it.

### Rationale
- Different rate of change (business rules vs. provider APIs)
- Different failure modes (wrong timing vs. delivery failure)
- Each fits comfortably in a context window independently
- Combined, they exceed comfortable context budget with tests

### Consequences
- Need a clear ReminderEvent contract between them
- Must handle "scheduled but not delivered" as an explicit state
```

---

## Ownership Analysis

### What's Actually Happening When a Human Does This

```
1. Understand the problem shape       → "This is really three sub-problems"
2. Assess team structure              → "We have two teams, so two services"
3. Predict rate of change             → "Pricing rules change monthly, delivery rarely"
4. Apply Conway's Law intentionally   → "Align components to team ownership"
5. Manage technical debt/history      → "Don't touch the legacy auth system"
6. Estimate context budgets           → "This unit is too big for one AI session"
```

### What Would AI Need to Own This

| Sub-task | Barrier | Type |
|---|---|---|
| Understand problem shape | AI can analyze contracts and data models for coupling/cohesion | **Solvable today** — this is graph analysis |
| Assess team structure | AI needs org chart, team skills, capacity, availability | **Access problem** — just not provided |
| Predict rate of change | AI can analyze git history, ticket velocity, business domain volatility | **Solvable today** — better than human intuition if given the data |
| Apply Conway's Law | Needs to understand social dynamics, team relationships, communication patterns | **Partially solvable** — can model org structure, can't model politics |
| Manage technical debt | Needs codebase history, known fragilities, institutional knowledge of "don't touch that" | **Tacit knowledge** — but could be externalized as constraints |
| Estimate context budgets | Token counting is trivial for AI | **Already AI-native** |

### The Honest Assessment

**Decomposition is surprisingly close to AI-ownable.** The technical aspects (coupling analysis, cohesion metrics, context budgets, dependency graphing) are *already better done by machines*. The blockers are:

1. **Organizational context isn't provided.** AI doesn't know your team structure, who's on vacation, who's an expert in what, or that Team B and Team C can't work together because of a reorg last year.

2. **"Don't touch that" knowledge is tacit.** Every codebase has sacred cows and landmines. These aren't documented because "everyone knows." AI can't know what nobody wrote down.

3. **Decomposition is a betting decision.** You're betting on which boundaries will hold over time. That requires predicting the future of the business, which is a judgment call.

---

## Irreducible Human Input

```
IRREDUCIBLE:
├── Organizational constraints: "Team A owns payments, Team B owns notifications"
├── Sacred cows: "Do not modify the legacy auth service"
├── Strategic bets: "We think pricing will change a lot, delivery won't"
└── Approval of final decomposition

AI-GENERATABLE:
├── Coupling/cohesion analysis of existing code
├── Context window budget calculations
├── Dependency graph generation
├── Decomposition proposals ranked by predicted stability
├── Rate-of-change predictions from historical data
└── Boundary suggestions based on DDD heuristics
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Decide where to draw boundaries | Approves & constrains | Proposes based on coupling analysis |
| Estimate context budgets | Validates | Counts tokens, flags oversize units |
| Identify shared concepts | Reviews | Scans for repeated terms and structures |
| Write boundary decision records | Authors the "why" | Drafts the template and rationale |
| Map dependencies | Validates | Derives from imports/contracts |
| Predict rate of change | Provides strategic context | Analyzes historical data |
