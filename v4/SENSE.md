# Phase 1: SENSE

> **"What's happening out there — and what signals matter?"**

## Purpose

Gather signals from the environment before making decisions. This is Boyd's "Observe" — but scoped to alignment-relevant signals only. Most signals are noise. SENSE filters for the ones that affect alignment decisions.

---

## Signal Categories

### Market Signals
```
├── Customer behavior changes (usage patterns, churn, feedback)
├── Competitor moves (new features, pricing changes)
├── Market conditions (regulation, economic shifts)
└── Technology shifts (new capabilities, deprecated platforms)
```

### System Signals
```
├── Production metrics (latency, errors, usage patterns)
├── Engineering metrics (DORA: lead time, deploy frequency, failure rate)
├── Context health (window utilization, spec clarity scores)
└── Alignment metrics (traceability gaps, contract drift)
```

### Team Signals
```
├── Team velocity and capacity changes
├── Knowledge distribution (bus factor, expertise gaps)
├── Morale and engagement indicators
└── Communication pattern changes
```

---

## Signal vs. Noise

The critical SENSE skill: distinguish signal from noise.

```
SIGNAL (changes an alignment decision):
├── "Users are opening reminders but not acting on them"
│    → Might mean: reminders work, but reschedule is the real problem
├── "Context window utilization for Unit X hit 85%"
│    → Might mean: decompose Unit X before it breaks
└── "Competitor launched one-tap reschedule"
     → Might mean: accelerate reschedule-flow priority

NOISE (interesting but doesn't change a decision):
├── "Page load time improved by 50ms"
│    → Nice, but not related to missed appointments outcome
├── "New JavaScript framework released"
│    → Doesn't change what we're building or why
└── "10 new GitHub stars this week"
     → Vanity metric, no alignment impact
```

### The Decision Test

For every signal, ask: **"Would this change any decision I'm about to make?"**

- If yes → it's a signal. Route to FRAME.
- If no → it's noise. Log it but don't act.

---

## AI's Role in SENSE

AI is the **primary sensor**. It monitors, collects, filters, and surfaces:

```
AI MONITORS:                          AI SURFACES TO HUMANS:
────────────                          ─────────────────────
All production metrics                Anomalies and threshold breaches
All user behavior data                Trend changes and inflection points
All engineering metrics               Signals that match decision criteria
Competitor public activity            Pattern breaks
Spec clarity scores                   Items failing the decision test
Context window utilization            Budget warnings
```

### Sensing Prompt Pattern

```
"Given the current outcome definition and active specs,
review the following data and identify:
1. Signals that might change an alignment decision
2. Contradictions between our assumptions and the data
3. Trends that will force a decision in the next 2 weeks

Data: [metrics, usage data, feedback]"
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define what signals matter | Sets criteria | Monitors against criteria |
| Collect raw data | Nothing (fully offloaded) | Collects from all sources |
| Filter signal from noise | Reviews AI's assessment | Applies decision test |
| Interpret ambiguous signals | Judges what it means | Provides context and history |
| Route signals to FRAME | Decides priority | Suggests urgency ranking |
