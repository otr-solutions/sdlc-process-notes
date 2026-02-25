# Traceability Model: The System's Complete History

> **"The graph tracks current state. Traceability tracks everything that ever happened."**

This document defines the event sourcing and audit log model that records every state change, decision, and cascade across all core models. Without traceability, the graph is a snapshot with no memory — it can tell you what's true now, but not how it got there.

This is **Spine B** in the architecture. Metaphor: **The memory — the system's complete history.**

**Cross-references**: [GRAPH_MODEL.md](./GRAPH_MODEL.md) · [STATE_MODEL.md](./STATE_MODEL.md) · [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) · [BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md) · [INTERACTION_MODEL.md](./INTERACTION_MODEL.md)

---

## The Problem — What We Don't Get For Free

The current model tracks *current state*. Ask it "At Time 5, what did S3 Spec look like?" and it can't answer — the artifact has been overwritten. Four specific questions the current model cannot answer:

**1. Decision Traceability: "Why is S3 Spec designed this way?"**

To answer this, you need to trace the decision chain: S3 Spec ← L2 Decomposition ← L1 Outcome ← D3 Bet ← D2 Frame ← D1 Situation Assessment. Each of those nodes fed into S3 Spec at specific versions. But node artifacts get overwritten on update — the chain of reasoning is lost.

**2. Learning Traceability: "What did we learn about user churn?"**

D3 Bet went through three versions: fitness 0.4 (wrong), 0.7 (closer), 0.9 (validated). The learning trajectory — what changed, why each revision was made — tells you everything about how the team's understanding evolved. But only the current version (0.9) survives in the graph. The path to get there is gone.

**3. Cascade Traceability: "What caused S4 Make to go FAILING?"**

S4 Make failed because L1 Outcome changed, which cascaded through D3 Bet → S1 Intent → S3 Spec → S4 Make. But when you look at the graph now, the cascade history isn't there — you need to know which artifact versions were active at the time of the cascade, not the current versions.

**4. Portfolio Traceability: "Why did we kill Initiative A?"**

The INITIATIVE_KILLED event happened. But why? You need the full graph snapshot at decision time — what was the value map showing, what conflicts existed, who made the call, what alternatives were considered. Without this, portfolio decisions become folklore rather than institutional knowledge.

---

## The Approach — Event Sourcing with Materialized Views

```
EVENT LOG (source of truth):
  Every state transition recorded as an immutable event
  Captures: what changed, why, who, what triggered it, what cascaded

SNAPSHOTS (materialized views):
  Periodic graph snapshots for quick "point in time" queries
  Generated FROM the event log

CURRENT STATE (what we already have):
  The live graph — always the latest state
  Derived from events
```

The event log is append-only and immutable. The current graph state is a materialized view derived from replaying events. Snapshots are periodic materializations for query efficiency. This means:

- You can always reconstruct the graph at any point in time by replaying events up to that timestamp
- You can always find the cause of any state transition by following trigger chains
- You can aggregate patterns across events to learn about model fitness

---

## Event Schema

Every event recorded by the traceability spine follows this schema:

```
EVENT SCHEMA:
├── id              — unique event identifier
├── timestamp       — when it happened
├── type            — what kind of event
├── scope           — initiative | portfolio | singleton
├── initiative_id   — which initiative (if applicable)
│
├── WHAT CHANGED:
│   ├── subject_type    — node | edge | attribute | portfolio
│   ├── subject_id      — which node/edge/attribute
│   ├── from_state      — previous state
│   ├── to_state        — new state
│   └── artifact_delta  — what changed in the artifact
│
├── WHY IT CHANGED:
│   ├── trigger_type    — transition | cascade | pulse | manual | decision
│   ├── trigger_event_id — the event that caused this one (for cascades)
│   ├── trigger_rule    — which rule fired
│   ├── trigger_check   — CHECKS result if applicable
│   └── context         — freeform context (measurement data, fitness scores, etc.)
│
├── WHO CHANGED IT:
│   ├── actor_type      — human | ai | system
│   ├── actor_id        — username or agent identifier
│   ├── authority_basis — trust_level + blast_radius + reversibility
│   └── role            — contributor | initiative_lead | portfolio_manager | architect
│
└── WHAT IT AFFECTED:
    ├── cascade_events   — list of event IDs for downstream cascades
    ├── blast_radius     — computed blast radius at time of event
    └── affected_nodes   — list of nodes affected
```

---

## Event Types — Complete Taxonomy

### NODE EVENTS

| Event | Description |
|-------|-------------|
| `NODE_CREATED` | A node was instantiated for an initiative |
| `NODE_TRANSITION` | A node changed state (EMPTY→DRAFT, DRAFT→ACTIVE, etc.) |
| `NODE_ARTIFACT_UPDATE` | A node's artifact was updated without a state change |
| `NODE_ATTRIBUTE_CHANGE` | A node attribute changed (trust_level, fitness, bandwidth) |
| `NODE_RETIRED` | A node was retired (initiative completed or killed) |

### EDGE EVENTS

| Event | Description |
|-------|-------------|
| `EDGE_ACTIVATED` | An edge became ACTIVE (source node reached ACTIVE state) |
| `EDGE_DEGRADED` | An edge's quality attributes dropped below threshold |
| `EDGE_BROKEN` | An edge became BROKEN (source or target in unrecoverable state) |
| `EDGE_REPAIRED` | A broken or degraded edge was restored |
| `EDGE_QUALITY_UPDATE` | encode/transmit/decode quality attributes updated |
| `CHECKS_RESULT` | A CHECKS edge completed — pass, fail, or drift signal |

### CASCADE EVENTS

| Event | Description |
|-------|-------------|
| `CASCADE_TRIGGERED` | A state change triggered a cascade evaluation |
| `CASCADE_PROPAGATED` | A cascade was allowed to continue downstream |
| `CASCADE_CONTAINED` | A cascade was stopped (human decision, blast radius limit, authority check) |

### PORTFOLIO EVENTS

| Event | Description |
|-------|-------------|
| `INITIATIVE_PROPOSED` | A new initiative was proposed to the portfolio |
| `INITIATIVE_ACTIVATED` | A proposed initiative was approved and activated |
| `INITIATIVE_PAUSED` | An active initiative was paused (resource constraint, conflict) |
| `INITIATIVE_KILLED` | An initiative was terminated |
| `INITIATIVE_COMPLETED` | An initiative reached its outcome and was completed |
| `PRIORITY_CHANGED` | Initiative priority was reordered |
| `RESOURCE_REALLOCATED` | N1 Topology or A1 Focus resources were reallocated between initiatives |
| `CONFLICT_DETECTED` | A CONFLICTS edge was detected between two initiatives |

### TENSION EVENTS

| Event | Description |
|-------|-------------|
| `TENSION_ACTIVATED` | A TENSIONS edge became active (opposing nodes in conflict) |
| `TENSION_ESCALATED` | An unresolved tension exceeded threshold time |
| `TENSION_RESOLVED` | A human made a decision that resolved the tension |
| `TENSION_IGNORED` | A tension was explicitly set aside (risk acknowledged) |

### PULSE EVENTS

| Event | Description |
|-------|-------------|
| `PULSE_TRIGGERED` | A scheduled PULSE cadence fired |
| `PULSE_COMPLETED` | A PULSE run completed successfully |
| `SENSOR_REFRESHED` | A sensor successfully updated a node's data |
| `SENSOR_STALE` | A sensor failed to refresh within its expected cadence |
| `ACTUATOR_FIRED` | An actuator was triggered to push a change to the external world |

### BOUNDARY EVENTS

| Event | Description |
|-------|-------------|
| `SENSOR_CONNECTED` | A new sensor was attached to a node |
| `SENSOR_DISCONNECTED` | A sensor was removed or became unavailable |
| `ACTUATOR_SUCCEEDED` | An actuator successfully applied a change to reality |
| `ACTUATOR_FAILED` | An actuator failed to apply a change |
| `LOOP_CLOSED` | A sensor confirmed the effect of a prior actuator (feedback loop closed) |

---

## Query Patterns — What Traceability Enables

### 1. Decision Traceability

*"Why is S3 Spec designed this way?"*

→ Find all `NODE_TRANSITION` events for S3 Spec. For each, follow `trigger_event_id` backward through the FEEDS chain: S3 Spec ← L2 Decomposition ← L1 Outcome ← D3 Bet ← D2 Frame ← D1 Situation Assessment. Reconstruct the artifact at each step. The complete reasoning chain is now recoverable.

### 2. Learning Traceability

*"What did we learn about user churn?"*

→ Find all `CHECKS_RESULT` events for D3 Bet nodes related to churn. Follow each `CHECKS_RESULT` to the `NODE_TRANSITION` events it triggered on D4 Learn. The fitness score history (0.4 → 0.7 → 0.9) and the reasons for each revision are fully traceable.

### 3. Cascade Traceability

*"What caused S4 Make to go FAILING?"*

→ Find the `NODE_TRANSITION` event for S4 Make → FAILING. Follow `trigger_event_id` backward through the cascade chain. Reconstruct the artifact versions that were active at each step. The full chain — from root cause to terminal failure — is recoverable.

### 4. Portfolio Traceability

*"Why did we kill Initiative A?"*

→ Find the `INITIATIVE_KILLED` event. The event records: who decided (`actor_id`, `role`), the authority basis, the context (what value map showed, what conflicts existed). Use the event's `timestamp` to reconstruct the graph snapshot at that moment. The decision is fully documented.

### 5. Temporal Queries

*"What did the graph look like on Feb 15?"*

→ Find the nearest snapshot before Feb 15. Replay all events from the snapshot timestamp to Feb 15 23:59:59. The graph state at any point in time is exactly reconstructible.

### 6. Pattern Queries

*"How often do bets need revision?"*

→ Aggregate `NODE_ARTIFACT_UPDATE` and `NODE_TRANSITION` events for D3 Bet nodes across all initiatives. Count revisions per initiative, correlate with fitness scores, identify patterns. This feeds directly into [Meta-Evolution](./META_EVOLUTION_MODEL.md) — is the D3 Bet node being used correctly? Are the revision patterns suggesting the model needs adjustment?

### 7. Authority Queries

*"Is AI making good autonomous decisions?"*

→ Find all events where `actor_type = ai`. Correlate with downstream `CHECKS_RESULT` events. Where AI made decisions autonomously (T3+), did CHECKS confirm quality? Where quality was poor, what was the blast radius? This feeds trust calibration — should T3 trust be promoted or demoted?

---

## Where Traceability Threads Through Core Models

Traceability is a cross-cutting spine — it doesn't belong to any single core model. It records events from every model simultaneously:

| Core Model | What Traceability Records |
|-----------|-------------------------|
| [Graph](./GRAPH_MODEL.md) | Node/edge creation, attribute changes, all NODE_EVENTS and EDGE_EVENTS |
| [State](./STATE_MODEL.md) | Every transition, every cascade trigger/propagation/containment |
| [Portfolio](./PORTFOLIO_MODEL.md) | Portfolio decisions, initiative lifecycle, resource allocation changes |
| [Boundary](./BOUNDARY_MODEL.md) | Sensor readings, actuator effects, feedback loop closures |
| [Interaction](./INTERACTION_MODEL.md) | Who made what decision, AI vs human actions, authority basis used |

---

## Relationship to Other Models + Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                   SYSTEM ARCHITECTURE                            │
│                                                                  │
│   CORE MODELS                    CROSS-CUTTING SPINES            │
│   (dependency ordered)           (thread through all)            │
│                                                                  │
│   ┌─────────────────────┐       ┌──────────────────────────┐    │
│   │ 5. Interaction      │       │ A. Dimensions (v1-v9)    │    │
│   │    "How participants│◀══════│    "The knowledge"       │    │
│   │     engage"         │       │                          │    │
│   ├─────────────────────┤       ├──────────────────────────┤    │
│   │ 4. Boundary         │       │ B. Traceability          │    │
│   │    "How it connects │◀══════│    "The memory"          │    │
│   │     to reality"     │       │    ◀── YOU ARE HERE      │    │
│   ├─────────────────────┤       ├──────────────────────────┤    │
│   │ 3. Portfolio        │       │ C. Implementation        │    │
│   │    "How many        │◀══════│    "The engineering"     │    │
│   │     interact"       │       │                          │    │
│   ├─────────────────────┤       ├──────────────────────────┤    │
│   │ 2. State            │       │ D. Meta-Evolution        │    │
│   │    "How it moves"   │◀══════│    "The learning"        │    │
│   │                     │       │                          │    │
│   ├─────────────────────┤       └──────────────────────────┘    │
│   │ 1. Graph            │                                       │
│   │    "What exists"    │       Each spine connects to EVERY    │
│   │                     │       core model, not just the one    │
│   └─────────────────────┘       shown with the arrow            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

| Type | # | Model | Metaphor | Document |
|------|---|-------|----------|----------|
| Core | 1 | Graph | The anatomy | [GRAPH_MODEL.md](./GRAPH_MODEL.md) |
| Core | 2 | State | The physiology | [STATE_MODEL.md](./STATE_MODEL.md) |
| Core | 3 | Portfolio | The ecology | [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) |
| Core | 4 | Boundary | The senses | [BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md) |
| Core | 5 | Interaction | The interface | [INTERACTION_MODEL.md](./INTERACTION_MODEL.md) |
| Spine | A | Dimensions | The knowledge | v1-v9 folders + [DIMENSION_INDEX.md](./DIMENSION_INDEX.md) |
| Spine | B | **Traceability (this document)** | **The memory** | **TRACEABILITY_MODEL.md** |
| Spine | C | Implementation | The engineering | [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) |
| Spine | D | Meta-Evolution | The learning | [META_EVOLUTION_MODEL.md](./META_EVOLUTION_MODEL.md) |

**The key insight**: Core models have a dependency order (each depends on those below it). Spines are genuinely cross-cutting — they thread through all core models simultaneously.

- Core models answer: "What is the system?"
- Spines answer: "How does the system operate across all models?"
