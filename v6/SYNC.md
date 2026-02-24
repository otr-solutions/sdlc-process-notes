# Domain 4: SYNC

> **"When do people need to align — and what's the minimal effective ritual?"**

## Purpose

Synchronization points are where humans verify alignment with each other. They're expensive (human attention, coordination cost, meeting overhead) but necessary. SYNC designs the **minimal set of rituals** that keep alignment intact without wasting attention.

---

## The Synchronization Spectrum

```
CONTINUOUS ◄──────────────────────────────────► PERIODIC
(always aligned)                                (checkpoint-aligned)

Pair programming    Daily standup    Sprint review    Quarterly planning
Real-time collab    Async review     Integration demo Outcome reassessment
Mob programming     PR review        Retrospective    Strategy offsite

High cost           Medium cost      Low cost         Lowest cost
High bandwidth      Medium bandwidth Lower bandwidth  Lowest bandwidth
Best for: Complex   Best for: Flow   Best for: Check  Best for: Course-correct
```

### The Minimum Viable Sync Set

```
DAILY:    Alignment standup (5 min)
          "What alignment decision am I facing today?"
          NOT: status update (AI dashboards handle this)

WEEKLY:   Integration review (30 min)
          "Do the seams between units hold?"
          Review AI-flagged integration issues

PER-MILESTONE: Outcome check (1 hour)
          "Are metrics moving? Should we pivot?"
          Review against Layer 1 outcome definition

QUARTERLY: Strategy sync (half-day)
          "Are outcomes still the right outcomes?"
          Reassess the entire opportunity tree
```

---

## AI-Enabled Sync

AI reduces synchronization overhead:

### Pre-Sync
```
Before any sync meeting, AI prepares:
├── Summary of changes since last sync
├── Flagged alignment risks
├── Metric movements
├── Outstanding decisions needing human input
└── Proposed agenda (highest-risk items first)

Human time spent preparing: 0 minutes
Human time in meeting: Focused on decisions, not updates
```

### During-Sync
```
AI can participate in sync as:
├── Real-time fact checker ("the spec actually says X")
├── Option generator ("three ways to resolve this")
├── Note taker (captures decisions and rationale)
└── Impact analyzer ("if we change this, it affects units X, Y, Z")
```

### Post-Sync
```
After sync, AI:
├── Updates decision records with new decisions
├── Updates specs/contracts based on decisions
├── Notifies affected teams/units
├── Schedules follow-up decisions
└── Updates dashboards to reflect new direction
```

---

## Async-First, Sync-When-Needed

```
DEFAULT:   Communicate through shared artifacts (async)
           AI mediates — loads context, generates, updates

ESCALATE TO SYNC when:
├── Artifact-mediated communication failed (misunderstanding persisted)
├── Decision requires real-time negotiation (competing values)
├── Emergent behavior at seams needs collaborative judgment
├── Relationship trust needs maintenance (human bonding)
└── Crisis requires coordinated response
```

---

## Sync Anti-Patterns

### 1. Status Meeting Syndrome
```
SYMPTOM:  Meeting is 45 minutes of "what I did yesterday"
FIX:      AI generates status dashboards. Meeting focuses ONLY on alignment decisions.
          If no alignment decisions exist → cancel the meeting.
```

### 2. Over-Syncing
```
SYMPTOM:  Daily meetings, weekly meetings, bi-weekly meetings, monthly meetings...
FIX:      Audit each ritual: "What alignment decision does this meeting serve?"
          If no clear answer → eliminate or reduce frequency.
```

### 3. Under-Syncing
```
SYMPTOM:  Teams drift apart. Integration reveals misalignment after weeks of work.
FIX:      AI-triggered sync: when integration tests fail or contract drift detected,
          AI requests a sync meeting with specific agenda.
```

### 4. Wrong-Level Syncing
```
SYMPTOM:  Executives discussing implementation details; developers discussing strategy
FIX:      Match sync participants to decision level:
          T1 decisions → leadership
          T2 decisions → team leads
          T3+ decisions → don't need sync (AI handles)
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Design sync cadence | Sets ritual frequency | Proposes based on alignment risk |
| Prepare for sync | Shows up focused | Generates pre-read, agenda, flags |
| Conduct sync | Makes alignment decisions | Provides facts, options, impact analysis |
| Post-sync follow-through | Validates AI's updates | Updates all artifacts, notifies teams |
| Audit sync effectiveness | Judges whether rituals work | Tracks time-in-meeting vs. decisions-made ratio |
| Trigger emergency sync | Responds to AI escalation | Detects when async alignment has failed |
