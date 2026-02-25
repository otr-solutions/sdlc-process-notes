# T1: Human Only

> **AI provides information. Humans own the decision entirely.**

## When to Use

```
├── The decision is IRREVERSIBLE or extremely costly to reverse
├── The decision requires VALUE JUDGMENTS (what matters more?)
├── The decision requires AUTHORITY (someone must be accountable)
├── The decision is NOVEL (no precedent, no pattern to follow)
└── The decision has ETHICAL implications
```

## Activities at This Level

```
├── Outcome definition: "What are we trying to change in the world?"
├── Value ranking: "Reliability > performance > cost"
├── Prioritization: "This matters more than that"
├── Risk appetite: "We'll accept 20% chance of failure"
├── Ethical calls: "This feature could harm user X; is it worth it?"
├── Accountability: "I own this result"
└── Organizational constraints: "Team B owns payments"
```

## AI's Role (Information Only)

AI provides raw material for the decision but does NOT propose or recommend:

```
AI provides:                    AI does NOT do:
────────────                    ──────────────
Raw data and trends             Propose outcomes
Stakeholder feedback summary    Recommend priorities
Competitive landscape           Make value judgments
Historical patterns             Assign accountability
Risk modeling scenarios         Decide ethical trade-offs
```

## Context Window Implications

At T1, context windows serve human cognition, not AI generation:

```
Load into AI context:
  - All relevant data for the decision
  - Historical context
  - Stakeholder perspectives

AI output: Structured summary for human consumption
  - Not a recommendation — a landscape
  - "Here's what we know. Here's what's uncertain. You decide."
```

## Graduation Criteria (T1 → T2)

An activity moves to T2 when:
```
├── The decision type has been made 3+ times before
├── A pattern has emerged that AI could propose from
├── The stakes have decreased (reversibility increased)
└── The human is confident in their ability to evaluate AI proposals
```

## Anti-Pattern: Keeping Everything at T1

```
SYMPTOM:  "I need to personally decide everything"
COST:     Human becomes the bottleneck; AI's capabilities are wasted
FIX:      Audit the T1 list quarterly — what's become routine?
          Routine T1 decisions should graduate to T2
```
