# Boundary Model: How the Graph Connects to Reality

> **"The graph without sensors is fiction."**

This document defines how the graph connects to reality — the membrane between the model and the external world. Without this, the graph is a closed system that can't sense reality or affect it.

This is **Layer 5** in the 8-layer architecture stack. Metaphor: **The senses — how the system perceives and affects its environment.**

**Cross-references**: [GRAPH_MODEL.md](./GRAPH_MODEL.md) · [STATE_MODEL.md](./STATE_MODEL.md) · [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md)

---

## The Boundary Concept

The system has a clear "inside" (the graph, states, portfolio) and needs a clear boundary defining what's inside vs outside.

```
                    THE BOUNDARY

  REALITY                │              THE GRAPH
  (external world)       │              (our model)
                         │
  Code repos ──SENSOR───▶│──▶ M2 Territory
  Analytics ───SENSOR───▶│──▶ L6 Measurement
  User behavior─SENSOR──▶│──▶ M4 Journey
  Team data ───SENSOR───▶│──▶ N1 Topology
  Market data ─SENSOR───▶│──▶ D1 Sense
  Time tracking─SENSOR──▶│──▶ A1 Focus
                         │
  Code repos ◀─ACTUATOR──│◀── S4 Make
  Teams ◀──────ACTUATOR──│◀── Portfolio Decisions
  Dashboards ◀─ACTUATOR──│◀── L6 Measurement
  Comms tools ◀─ACTUATOR─│◀── State Changes
                         │
           ◀─FEEDBACK LOOP──▶
           (sensors + actuators
            forming closed loops)
```

Sensors flow inward — they keep the graph's state aligned with the real world. Actuators flow outward — they change the real world based on graph state and decisions. Feedback loops close the circuit.

---

## Sensors — How the Graph Perceives Reality

Sensors are inbound data feeds that keep the graph's state aligned with the real world.

### M2 Territory Map sensors

| Sensor | Source → Target |
|--------|-----------------|
| CODE_HEALTH | Static analysis → code quality scores |
| DEPENDENCY_HEALTH | Dependency scanners → vulnerability/freshness |
| CHANGE_VELOCITY | Git analysis → change frequency, ownership patterns |
| INFRASTRUCTURE_STATE | Monitoring → uptime, resource utilization |
| DEPLOYMENT_STATE | CI/CD → what's deployed where, deploy frequency |
| DEBT_SIGNALS | Tech debt tracking → accumulation rate, hotspots |

### M1 Value Map sensors

| Sensor | Source → Target |
|--------|-----------------|
| REVENUE_SIGNALS | Business metrics → revenue by feature/product area |
| USAGE_SIGNALS | Analytics → feature adoption, engagement depth |
| COST_SIGNALS | Infrastructure costs → cost per feature/transaction |
| MARKET_SIGNALS | Competitive analysis → market position shifts |

### M4 Journey Map sensors

| Sensor | Source → Target |
|--------|-----------------|
| BEHAVIOR_SIGNALS | Product analytics → funnel completion, drop-off points |
| EXPERIENCE_SIGNALS | Session replay → friction points, confusion patterns |
| SUPPORT_SIGNALS | Support tickets → pain point frequency, severity |
| SATISFACTION_SIGNALS | NPS/CSAT surveys → satisfaction trends |

### M3 Context Map sensors

| Sensor | Source → Target |
|--------|-----------------|
| COUPLING_SIGNALS | Code analysis → cross-domain dependencies |
| API_SIGNALS | API analytics → interface usage patterns, breaking changes |
| TEAM_SIGNALS | Communication patterns → cross-team interaction frequency |

### N1 Topology sensors

| Sensor | Source → Target |
|--------|-----------------|
| CAPACITY_SIGNALS | Sprint/cycle data → actual vs planned capacity |
| AVAILABILITY_SIGNALS | Calendar/PTO → team availability |
| LOAD_SIGNALS | PR queues, review times → current workload |
| HEALTH_SIGNALS | Team surveys → morale, burnout indicators |

### A1 Focus sensors

| Sensor | Source → Target |
|--------|-----------------|
| ATTENTION_SIGNALS | Meeting load, context switches → actual attention spend |
| INTERRUPT_SIGNALS | On-call, incidents → unplanned attention drain |
| QUEUE_SIGNALS | Review queues, decision backlogs → attention debt |

### L6 Measurement sensors (per initiative)

| Sensor | Source → Target |
|--------|-----------------|
| OUTCOME_SIGNALS | Business metrics → outcome KPIs |
| QUALITY_SIGNALS | Error rates, bug reports → quality indicators |
| ADOPTION_SIGNALS | Usage metrics → adoption/rollout progress |
| PERFORMANCE_SIGNALS | Latency, throughput → performance indicators |

### D1 Situation Assessment sensors

| Sensor | Source → Target |
|--------|-----------------|
| MARKET_SIGNALS | Competitive intelligence → landscape changes |
| CUSTOMER_SIGNALS | Customer feedback → need/pain shifts |
| TECHNOLOGY_SIGNALS | Tech landscape → new capabilities/threats |
| REGULATORY_SIGNALS | Compliance requirements → new constraints |

---

## Sensor Properties

Every sensor has four properties that determine its usefulness:

| Property | Values | Meaning |
|----------|--------|---------|
| freshness | REALTIME, PERIODIC, ON_DEMAND | How current is the data? |
| reliability | MEASURED, REPORTED, INFERRED | How trustworthy is the data? |
| coverage | COMPLETE, SAMPLED, BIASED | How much of reality does it capture? |
| cost | FREE, CHEAP, EXPENSIVE | What does it take to maintain? |

**The quality chain principle**: Sensor quality directly affects graph state quality. If M2 Territory Map is fed by unreliable or stale sensors, the territory map is unreliable — and everything that FEEDS from it inherits that unreliability.

```
Sensor quality
  → node quality (M2, M1, M4, M3, N1, A1, L6, D1)
  → edge quality (FEEDS edges downstream)
  → decision quality (D1, D2, D3, portfolio decisions)
```

---

## Actuators — How the Graph Affects Reality

Actuators are outbound effects that change the real world based on graph state and decisions.

### Code Actuators (from S4 Make / L4 Generation)

| Actuator | Effect |
|----------|--------|
| PR_CREATION | Create/update pull requests |
| BRANCH_MANAGEMENT | Create/merge/delete branches |
| DEPLOYMENT_TRIGGER | Trigger CI/CD pipelines |
| FEATURE_FLAGS | Enable/disable feature flags |
| ROLLBACK | Revert deployments when FAILING detected |

### Communication Actuators (from decisions and state changes)

| Actuator | Effect |
|----------|--------|
| TEAM_NOTIFICATION | Alert teams to state changes |
| DECISION_REQUEST | Route tension/contested states to decision makers |
| STATUS_UPDATE | Push initiative state to dashboards/reports |
| STAKEHOLDER_ALERT | Notify stakeholders of portfolio changes |
| ESCALATION | Route high-blast-radius decisions to appropriate authority |

### Resource Actuators (from portfolio decisions)

| Actuator | Effect |
|----------|--------|
| ASSIGNMENT_UPDATE | Update team assignments in planning tools |
| CAPACITY_ALLOCATION | Adjust sprint/cycle capacity assignments |
| BUDGET_ADJUSTMENT | Reflect resource reallocation in financial systems |
| PRIORITY_SIGNAL | Update priority indicators across tools |

### Measurement Actuators (from L6 Measurement)

| Actuator | Effect |
|----------|--------|
| DASHBOARD_UPDATE | Push metrics to dashboards |
| ALERT_TRIGGER | Fire alerts when metrics cross thresholds |
| EXPERIMENT_CONTROL | Adjust A/B test parameters |
| FEEDBACK_LOOP | Route measurement results back to D4 Learn |

---

## Actuator Properties

| Property | Values | Meaning |
|----------|--------|---------|
| latency | IMMEDIATE, DELAYED, EVENTUAL | How quickly does the effect happen? |
| reversibility | REVERSIBLE, COSTLY_REVERSE, IRREVERSIBLE | Can the effect be undone? |
| blast_radius | NARROW, BROAD, SYSTEMIC | How much of reality does it change? |
| observability | OBSERVABLE, DELAYED_OBSERVABLE, UNOBSERVABLE | Can we see if it worked? |

**Note**: Actuator properties map directly to the authority model. IRREVERSIBLE + SYSTEMIC + UNOBSERVABLE = highest human authority required. This is why KILL decisions are always human.

---

## Feedback Loops — The 6 Named Loops

Closed loops where decisions → actuators → reality changes → sensors detect → graph updates → confirms or challenges the decision.

### 1. THE OUTCOME LOOP (strategic — weeks/months)

```
L1 Outcome → execution → deployment
  → OUTCOME_SIGNALS
  → L6 Measurement → D4 Learn
  → "Was the outcome achieved?"
```

### 2. THE BET LOOP (tactical — days/weeks)

```
D3 Bet → S1 Intent → S4 Make → experiment deployed
  → BEHAVIOR_SIGNALS
  → L6 Measurement → D4 Learn
  → "Was the bet right? Fitness updates."
```

### 3. THE TERRITORY LOOP (continuous)

```
M2 Territory → S4 Make changes code
  → CODE_HEALTH sensors detect
  → M2 Territory checks against reality
  → STALE if drifted
  → PULSE triggers refresh
  → RIPPLE to all nodes that FEEDS from M2
```

### 4. THE ATTENTION LOOP (weekly)

```
A1 Focus allocation → teams work on assigned nodes
  → ATTENTION_SIGNALS + INTERRUPT_SIGNALS
  → actual attention measured
  → A1 checks: is actual matching allocated?
  → rebalance if not
```

### 5. THE SIGNAL QUALITY LOOP (per-edge, continuous)

```
Upstream node fires FEEDS edge
  → downstream attempts to use
  → confusion/rework = low quality
  → edge quality measured
  → if below threshold → DEGRADED → P4 Correct
  → if fails → BROKEN → RIPPLE
```

### 6. THE PORTFOLIO VALUE LOOP (monthly)

```
Portfolio decisions → initiatives execute
  → REVENUE_SIGNALS + USAGE_SIGNALS
  → L6 across all initiatives
  → cross-initiative analysis
  → PRIORITIZE/KILL/SPAWN decisions
```

---

## Boundary Health

Four signals that indicate the boundary is healthy:

1. **Sensor freshness** — are sensors delivering current data? (stale sensors = stale maps = stale decisions)
2. **Actuator effectiveness** — are effects landing as intended? (unobservable actuators are dangerous)
3. **Loop closure time** — how long from action → measurement → learning? (long loops = slow adaptation)
4. **Fiction risk** — when sensors go stale, the graph diverges from reality. The system continues operating on a model that no longer reflects the world.

```
FICTION RISK: FLOW continues on stale sensor data
  = The map says one thing, reality is something else
  = Every decision made on the stale map is potentially wrong
  = The longer sensors are stale, the deeper the fiction
```

---

## Full Architecture Stack

| Layer | Model | Metaphor | Document |
|-------|-------|----------|----------|
| 8 | Meta-Evolution | The evolution | (future) |
| 7 | Implementation | The engineering | [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) |
| 6 | Interaction | The interface | [INTERACTION_MODEL.md](./INTERACTION_MODEL.md) |
| **5** | **Boundary (this document)** | **The senses** | **BOUNDARY_MODEL.md** |
| 4 | Portfolio | The ecology | [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) |
| 3 | State | The physiology | [STATE_MODEL.md](./STATE_MODEL.md) |
| 2 | Graph | The anatomy | [GRAPH_MODEL.md](./GRAPH_MODEL.md) |
| 1 | Dimensions (v1-v9) | The knowledge | v1-layered/ through v9-evolutionary/ |

---

## Relationship to Other Models

| Document | Metaphor | What It Defines |
|----------|----------|----------------|
| [GRAPH_MODEL.md](./GRAPH_MODEL.md) | **Anatomy** — what exists | 18 nodes, 6 edge types, attributes, operations |
| [STATE_MODEL.md](./STATE_MODEL.md) | **Physiology** — how it moves | States, transitions, authority, cascades, movement types |
| [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) | **Ecology** — how multiple organisms interact | Instance vs singleton nodes, cross-initiative edges, portfolio decisions |
| **BOUNDARY_MODEL.md** (this document) | **The senses** — how it perceives and affects reality | Sensors, actuators, feedback loops, boundary health |
| [INTERACTION_MODEL.md](./INTERACTION_MODEL.md) | **The interface** — how participants engage | Roles, views, AI patterns, trust-graduated collaboration |
| [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) | **The engineering** — how it's built | Graph runtime, node/edge embodiment, event system |

The boundary model is the **sensory layer**. It defines what the graph can perceive (sensors) and what it can affect (actuators). Without it, the graph is a closed system — structurally complete but disconnected from the world it's supposed to model.
