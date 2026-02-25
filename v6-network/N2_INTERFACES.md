# Domain 2: INTERFACES

> **"How do teams and AI communicate — and in what format?"**

## Purpose

Define the **protocols** for communication between teams, between humans and AI, and between AI sessions. Just as software components need APIs, organizational components need defined interfaces.

---

## Interface Types

### Human ←→ Human Interfaces
```
FORMAL:
├── Design docs / RFCs: Async, structured, reviewable
├── ADRs: Capture decisions and rationale
├── Contract reviews: Structured alignment checkpoints
└── Sprint/iteration reviews: Demo outcomes, not just features

INFORMAL:
├── Slack/chat: Quick clarification (not decisions)
├── Pairing sessions: Real-time shared cognition
└── Whiteboard: Spatial reasoning, emergent understanding
```

### Human ←→ AI Interfaces
```
INPUT (Human → AI):
├── Specs/contracts: Structured intent, machine-parseable
├── Prompts: Natural language, but with loaded context
├── Corrections: "That's not what I meant" + explanation
└── Constraints: Anti-goals, guardrails, conventions

OUTPUT (AI → Human):
├── Generated artifacts: Code, tests, docs
├── Summaries: Compressed information for human review
├── Flags: Anomalies, risks, questions
└── Options: Multiple proposals with trade-offs
```

### AI ←→ AI Interfaces (cross-session)
```
Shared artifacts that persist between AI sessions:
├── Glossary: Consistent terminology across all sessions
├── Schemas: Data structure definitions
├── Contracts: Interface agreements between units
├── Decision log: Previous decisions and rationale
└── Learning log: What's been validated/invalidated

These artifacts ARE the interface. There's no "AI-to-AI communication" —
there are shared documents that multiple AI sessions read.
```

---

## The Interface Contract Pattern

For every team boundary, define:

```markdown
## Interface: Team A (Scheduling) → Team B (Delivery)

ARTIFACT:     ReminderEvent
FORMAT:       JSON, schema in shared/reminder-event.schema.json
PRODUCER:     Team A's reminder-scheduling unit
CONSUMER:     Team B's notification-delivery unit
CONTRACT:     Consumer-driven (Team B defines what it needs)

COMMUNICATION PROTOCOL:
  Changes to interface:  RFC → both teams review → approve → implement
  Questions:             Shared Slack channel #scheduling-delivery
  Escalation:            Weekly sync if async fails

CONTEXT WINDOW IMPACT:
  Team A loads: own specs + ReminderEvent schema (not delivery internals)
  Team B loads: own specs + ReminderEvent schema (not scheduling internals)
```

---

## Consumer-Driven Contracts

Fowler's insight: **the consumer defines what it needs**, not the producer. This reduces interface bloat and keeps context windows small:

```
PRODUCER-DRIVEN (bloated):
  Team A publishes everything it knows about a reminder.
  20 fields, 3 nested objects, 500 tokens.
  Team B uses 5 fields.

CONSUMER-DRIVEN (lean):
  Team B says: "I need appointmentId, userId, channel, scheduledFor, priority."
  Team A produces exactly that.
  5 fields, ~100 tokens.
  Context overhead: 80% smaller.
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define interface boundaries | Decides based on team/domain structure | Proposes from dependency analysis |
| Author interface contracts | Cross-team negotiation | Drafts from schema analysis |
| Enforce interface discipline | Reviews cross-boundary changes | Validates produced output matches contract |
| Maintain shared artifacts | Reviews for accuracy | Updates glossary, schemas, contracts |
| Detect interface violations | Reviews escalations | Monitors for schema drift, breaking changes |
