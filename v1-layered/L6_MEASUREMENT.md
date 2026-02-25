# Layer 6: Measurement

> **Ownership: AI-Maintained, Human-Judged**

## Purpose

Close the loop. Measure whether the software is **actually driving the outcome** defined in Layer 1, and feed that signal back into the system.

---

## What Gets Measured

### 1. Outcome Metrics (from Layer 1)

```
Metric: Missed appointment rate
Baseline: 23%
Target: <10%
Current: 16% (after 4 weeks)
Trend: ↓ declining, -1.2%/week
Status: ON TRACK but not yet at target
```

### 2. Engineering Metrics (DORA)

```
Deployment frequency:     12/week → are we iterating fast enough?
Lead time for changes:    2.1 days → can we respond to feedback?
Change failure rate:      3% → is generation quality high?
Mean time to recovery:    45 min → can we recover from mistakes?
```

### 3. Alignment Metrics (novel to this model)

```
Contract coverage:     18/20 units have contracts → 2 units are unspecified risk
Traceability gaps:     3 implementations don't trace to any outcome → potential waste
Contract drift:        2 contracts haven't been reviewed in 30 days → staleness risk
Regeneration rate:     85% of code is regenerated, not hand-edited → low drift risk
Review turnaround:     Avg 4 hours from generation to alignment review → acceptable
```

### 4. Context Health Metrics

```
Avg context utilization:    62% of window → healthy, room to reason
Units exceeding budget:     1 unit at 78% → candidate for decomposition
Shared context size:        11K tokens → growing, watch for bloat
Contract clarity score:     AI asked 0.3 clarifying questions per generation → contracts are clear
```

---

## The Feedback Loop

Layer 6 signals feed back into the system:

```
Outcome metrics off-target
  → Re-examine opportunity tree (Layer 1)
  → Are we solving the right problems?

High change failure rate
  → Contracts may be ambiguous (Layer 3)
  → Decomposition may be wrong (Layer 2)

Traceability gaps
  → Unauthorized work is happening
  → Re-align to outcome (Layer 1)

Context budget exceeded
  → Unit is too large (Layer 2)
  → Decompose further

Contract drift detected
  → Trigger alignment review (Layer 5)

Low regeneration rate
  → Teams are hand-editing instead of updating contracts
  → Process adherence issue
```

---

## Ownership Analysis

### What's Actually Happening in This Layer

```
1. Collect metrics from production, CI/CD, and project tools
2. Aggregate and trend the data
3. Flag anomalies and threshold breaches
4. Correlate signals across metric categories
5. Interpret what the numbers mean for the project
6. Decide what action to take
```

### What Would AI Need to Own This Fully

| Sub-task | Barrier | Type |
|---|---|---|
| Collect metrics | AI/tooling can do this today | **Already automated** |
| Aggregate and trend | Statistical analysis is AI-native | **Already automated** |
| Flag anomalies | Pattern detection is AI-native | **Already automated** |
| Correlate signals | AI can identify correlations across data sources | **Solvable today** |
| Interpret meaning | Requires understanding business context: "is 16% good enough to ship?" | **Values question** |
| Decide action | Requires authority and risk judgment: "do we pivot or push forward?" | **Authority + judgment** |

### The Honest Assessment

Measurement is **90% automatable today**. The collection, trending, flagging, and correlation are all machine tasks. The only human input needed is:

1. **"What do these numbers mean for us?"** — Interpretation requires business context
2. **"What should we do about it?"** — Action requires authority and judgment
3. **"Is this metric still the right one?"** — Meta-evaluation requires strategic thinking

---

## Irreducible Human Input

```
IRREDUCIBLE:
├── Interpreting metrics against business context
├── Deciding whether to act on signals (pivot, adjust, or stay the course)
├── Evaluating whether the metrics themselves are still valid
└── Accountability for outcomes

AI-GENERATABLE:
├── Metric collection and aggregation
├── Trend analysis and forecasting
├── Anomaly detection and alerting
├── Cross-metric correlation
├── Dashboard generation and maintenance
├── Suggested interpretations ("at current trend, target reached in 3 weeks")
└── Recommended actions ranked by impact
```

---

## AI's Role in Measurement

AI maintains all dashboards, collects metrics, flags anomalies, and suggests interpretations. But **humans decide what to do about it**.

```
AI says:  "Unit reschedule-flow context utilization is 78% and growing.
           3 new rules were added in the last sprint. At current rate,
           it will exceed budget in 2 sprints. Recommend decomposition."

Human decides: "Yes, split it" or "No, we'll accept the risk because
               this unit ships next week and won't grow further."
```

```
AI says:  "Missed appointment rate dropped to 16% but the rate of
           decline is slowing. Linear projection suggests we'll plateau
           at 12%, above the 10% target. The reminder open rate for
           push notifications is 8% vs 34% for SMS. Recommend shifting
           channel priority toward SMS."

Human decides: "Good analysis. Update the contract for channel
               selection to prefer SMS. But keep push as a fallback —
               some users don't have SMS."
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define which metrics matter | Decides | Suggests based on outcome definition |
| Collect and aggregate data | Spot-checks | Automates fully |
| Trend analysis | Reviews | Produces forecasts and projections |
| Anomaly detection | Reviews flags | Monitors and alerts |
| Cross-metric correlation | Interprets causality | Identifies statistical correlations |
| Interpret meaning | Judges | Suggests interpretations with confidence |
| Decide action | Decides and owns | Recommends ranked options |
| Update metric definitions | Decides | Flags when metrics become stale or irrelevant |
