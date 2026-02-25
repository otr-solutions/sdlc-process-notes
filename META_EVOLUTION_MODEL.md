# Meta-Evolution Model: The System Improving Itself

> **"Every other model describes how the SYSTEM works. This model describes how the MODELS improve."**

This document defines how the architecture evaluates and evolves itself. Meta-evolution asks not just "is the system healthy?" but "is the model of the system correct?"

This is **Spine D** in the architecture. Metaphor: **The learning — the system improving itself.**

**Cross-references**: [GRAPH_MODEL.md](./GRAPH_MODEL.md) · [STATE_MODEL.md](./STATE_MODEL.md) · [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) · [BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md) · [INTERACTION_MODEL.md](./INTERACTION_MODEL.md) · [TRACEABILITY_MODEL.md](./TRACEABILITY_MODEL.md) · [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md)

---

## Introduction

The graph tracks current state. The state model defines how transitions work. The portfolio model defines how initiatives coexist. These models describe *how the system operates*. But they don't ask: "Are these models right?"

Meta-evolution is the cross-cutting spine that evaluates every model and every other spine. It asks:

- **Are the 18 nodes the right nodes?** (Graph Model fitness)
- **Are the cascade rules accurate?** (State Model fitness)
- **Are portfolio decisions producing good outcomes?** (Portfolio Model fitness)
- **Are sensors sufficient?** (Boundary Model fitness)
- **Is trust graduating correctly?** (Interaction Model fitness)
- **Is the event schema capturing the right information?** (Traceability Spine fitness)
- **Is the tool mapping working?** (Implementation Spine fitness)

Just as [D3 Bet](./GRAPH_MODEL.md) has a fitness score derived from L6 Measurement, each model has a fitness score derived from meta-evolution.

---

## What Meta-Evolution Evaluates

| Model/Spine | Meta-Evolution Question | Signal Source |
|------------|------------------------|---------------|
| [Graph Model](./GRAPH_MODEL.md) | Are the 18 nodes the right nodes? Should we add/remove? | Pattern queries from [Traceability](./TRACEABILITY_MODEL.md) |
| [State Model](./STATE_MODEL.md) | Are transition rules accurate? Are cascade rules correct? | Cascade accuracy from [Traceability](./TRACEABILITY_MODEL.md) |
| [Portfolio Model](./PORTFOLIO_MODEL.md) | Are portfolio decisions producing good outcomes? | Portfolio outcome data |
| [Boundary Model](./BOUNDARY_MODEL.md) | Are sensors sufficient? Are feedback loops closing? | Sensor freshness, loop closure time |
| [Interaction Model](./INTERACTION_MODEL.md) | Are views useful? Is trust calibrated? | Authority queries from [Traceability](./TRACEABILITY_MODEL.md) |
| Dimensions (v1-v9) | Are the v1-v9 frameworks still valid? | Decision quality trends |
| [Traceability](./TRACEABILITY_MODEL.md) | Is the event schema capturing the right information? | Query success/failure patterns |
| [Implementation](./IMPLEMENTATION_MODEL.md) | Is the tool mapping working? | Operational metrics |

---

## The Fitness of the Model Itself

### Graph Model Fitness

Are nodes being used? Are edges firing? Are any nodes always EMPTY across many initiatives — suggesting they're unnecessary? Are there recurring patterns that suggest a node is missing?

- **Signal**: Frequency of node activation (from `NODE_TRANSITION` events via Traceability)
- **Low fitness signal**: A node that is EMPTY in >80% of initiatives suggests it may not belong in the core model
- **High fitness signal**: All nodes active, edges firing as expected, cascades resolving correctly

### State Model Fitness

Do cascade predictions match actual cascades? Are state transitions triggering correctly? Are TENSION resolutions happening within acceptable time windows?

- **Signal**: `CASCADE_TRIGGERED` vs `CASCADE_PROPAGATED` vs `CASCADE_CONTAINED` ratios; `TENSION_RESOLVED` vs `TENSION_IGNORED` ratios
- **Low fitness signal**: Cascades are frequently contained manually in ways the rules didn't predict — the rules are wrong
- **High fitness signal**: Cascade behavior matches model predictions; TENSIONS resolve at expected rates

### Portfolio Model Fitness

Are PRIORITIZE decisions leading to better outcomes than alternatives? Are KILL decisions justified in hindsight? Is the portfolio ecology working — are initiatives amplifying rather than conflicting?

- **Signal**: `INITIATIVE_COMPLETED` with high fitness outcomes vs `INITIATIVE_KILLED` rates; conflict detection timing
- **Low fitness signal**: Initiatives frequently conflict late (should have been detected earlier); KILL decisions look bad in hindsight
- **High fitness signal**: Conflicts detected early; killed initiatives would have continued to fail based on fitness trajectory

### Boundary Model Fitness

Are sensors fresh? Are actuators effective? Are feedback loops closing within expected time? Are there important external signals the current sensor set can't detect?

- **Signal**: `SENSOR_STALE` frequency; `LOOP_CLOSED` timing; `ACTUATOR_FAILED` rates
- **Low fitness signal**: M2 Territory Map consistently stale; feedback loops not closing; actuators failing
- **High fitness signal**: Sensors refreshing on cadence; actuators succeeding; loops closing within expected windows

### Interaction Model Fitness

Are tensions being resolved promptly? Is trust graduation happening — are AI agents moving from T2 to T3 as evidence accumulates? Are the defined views (My Work, Tension Queue, Signal Health, Portfolio View, Architect View) actually being used?

- **Signal**: Authority queries from Traceability; trust level history; tension resolution time
- **Low fitness signal**: Tensions aging without resolution; trust not graduating despite consistent AI quality; views not being used
- **High fitness signal**: Trust graduating based on evidence; tensions resolving promptly; humans using views to make decisions

---

## Evolution Mechanisms

The feedback loop for model improvement:

```
TRACEABILITY (provides the data)
    │
    │  Pattern queries, authority queries,
    │  cascade accuracy, fitness trajectories
    ▼
META-EVOLUTION (provides the evaluation)
    │
    │  "State Model cascade rules are wrong in X% of cases"
    │  "Graph Model is missing a node for Y pattern"
    │  "Trust graduation is too slow for Z node type"
    ▼
HUMAN JUDGMENT (provides the decision)
    │
    │  What to change, when, why
    ▼
MODEL UPDATE (changes to documents/rules)
    │
    │  Recorded as events in TRACEABILITY
    │  (the change to the model is itself traced)
    ▼
    └──────────────────────────────────────────▶ repeat
```

Changes to models are themselves recorded by traceability — the system's self-improvement is fully auditable.

---

## Current Status

This model is intentionally lightweight. Meta-evolution requires operational data to be meaningful — you need the system running, events accumulating, and patterns emerging before you can evaluate model fitness.

As the system operates and traceability data accumulates, this model will be expanded with:

- **Specific fitness metrics** for each model and spine (with thresholds)
- **Evaluation cadences** (how often each model's fitness is reviewed)
- **Evolution processes** (who decides what to change, how changes are tested)
- **Model versioning** (how to track model changes over time without losing the history)

The framework is here. The specific metrics will emerge from operating experience.

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
│   │     to reality"     │       │                          │    │
│   ├─────────────────────┤       ├──────────────────────────┤    │
│   │ 3. Portfolio        │       │ C. Implementation        │    │
│   │    "How many        │◀══════│    "The engineering"     │    │
│   │     interact"       │       │                          │    │
│   ├─────────────────────┤       ├──────────────────────────┤    │
│   │ 2. State            │       │ D. Meta-Evolution        │    │
│   │    "How it moves"   │◀══════│    "The learning"        │    │
│   │                     │       │    ◀── YOU ARE HERE      │    │
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
| Spine | B | Traceability | The memory | [TRACEABILITY_MODEL.md](./TRACEABILITY_MODEL.md) |
| Spine | C | Implementation | The engineering | [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) |
| Spine | D | **Meta-Evolution (this document)** | **The learning** | **META_EVOLUTION_MODEL.md** |

**The key insight**: Core models have a dependency order (each depends on those below it). Spines are genuinely cross-cutting — they thread through all core models simultaneously. Meta-evolution is the spine that evaluates all other spines and core models — it is the system's capacity for self-improvement.

- Core models answer: "What is the system?"
- Spines answer: "How does the system operate across all models?"
