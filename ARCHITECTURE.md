# Architecture: The AI Software Factory Operating System

> **"5 core models that define what the system is. 4 cross-cutting spines that define how it operates."**

This is the entry point and navigation map for the entire model architecture. Start here to understand how the documents fit together.

---

## System Architecture

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
| Spine | B | Traceability | The memory | [TRACEABILITY_MODEL.md](./TRACEABILITY_MODEL.md) |
| Spine | C | Implementation | The engineering | [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) |
| Spine | D | Meta-Evolution | The learning | [META_EVOLUTION_MODEL.md](./META_EVOLUTION_MODEL.md) |

---

## The Core Models

Core models define **what the system is**. They have a dependency order — each model depends on the models below it.

### Core Model 1: Graph — "What exists"
**[GRAPH_MODEL.md](./GRAPH_MODEL.md)** · Metaphor: The anatomy

The graph is the foundation. It defines 18 nodes (the things that have state and produce artifacts), 6 edge types (how nodes relate), 7 attributes (properties that change behavior), and 12 operations (behaviors that act on the graph). Everything else in the architecture is built on top of this structure.

### Core Model 2: State — "How it moves"
**[STATE_MODEL.md](./STATE_MODEL.md)** · Metaphor: The physiology

The state model defines how the graph moves — the 7 node states, 4 edge states, authority model, cascade rules, and 3 movement patterns (FLOW, RIPPLE, PULSE). The graph is the anatomy; the state model is the physiology.

*Depends on*: Graph Model

### Core Model 3: Portfolio — "How many interact"
**[PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md)** · Metaphor: The ecology

The portfolio model defines how multiple initiatives coexist in a shared reality — instance vs singleton nodes, cross-initiative edges, portfolio decisions (PRIORITIZE, SEQUENCE, RESOLVE, KILL, SPAWN), and the five portfolio cadences.

*Depends on*: Graph Model, State Model

### Core Model 4: Boundary — "How it connects to reality"
**[BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md)** · Metaphor: The senses

The boundary model defines how the graph connects to the external world — sensors (inbound data feeds that keep graph state aligned with reality) and actuators (outbound actions that change reality based on graph state), plus the feedback loops that close the circuit.

*Depends on*: Graph Model, State Model, Portfolio Model

### Core Model 5: Interaction — "How participants engage"
**[INTERACTION_MODEL.md](./INTERACTION_MODEL.md)** · Metaphor: The interface

The interaction model defines who interacts with the system and how — roles (Contributor, Initiative Lead, Portfolio Manager, Architect, AI Agent), five human views, AI participation patterns, and trust-graduated collaboration.

*Depends on*: All other core models

---

## The Cross-Cutting Spines

Spines define **how the system operates across all models**. They are genuinely cross-cutting — each spine threads through every core model simultaneously. Forcing spines into a layer stack was creating false linearity.

### Spine A: Dimensions — "The knowledge"
**[DIMENSION_INDEX.md](./DIMENSION_INDEX.md)** · Folders: v1-layered/ through v9-evolutionary/

The nine dimensions (v1 through v9) are the knowledge foundation. They contain deep reasoning about each facet of software delivery — how to decompose, how to frame, how to build trust, how to measure. The core models tell you *what to do*; the dimensions tell you *how to do it well*.

### Spine B: Traceability — "The memory"
**[TRACEABILITY_MODEL.md](./TRACEABILITY_MODEL.md)**

Traceability is the event sourcing model — the complete, immutable history of everything that ever happened across all core models. It records every state transition, cascade, portfolio decision, sensor reading, and human/AI action. Without traceability, the graph is a snapshot with no memory.

### Spine C: Implementation — "The engineering"
**[IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md)**

Implementation is the engineering spine that constrains and enables every core model. It maps abstract graph concepts to concrete tools (GitHub Issues, PRs, Actions, Markdown), defines the graph runtime data model, and describes how FLOW, RIPPLE, and PULSE are implemented in practice.

### Spine D: Meta-Evolution — "The learning"
**[META_EVOLUTION_MODEL.md](./META_EVOLUTION_MODEL.md)**

Meta-evolution asks whether the model itself is right. It evaluates the fitness of every core model and every other spine — are the 18 nodes the right nodes? Are cascade rules accurate? Are portfolio decisions producing good outcomes? It is the system's capacity for self-improvement.

---

## Core Model Dependency Order

```
5. Interaction  (depends on all below)
      │
4. Boundary     (depends on 1-3)
      │
3. Portfolio    (depends on 1-2)
      │
2. State        (depends on 1)
      │
1. Graph        (foundation — no dependencies)
```

You cannot understand the boundary model without understanding the portfolio model, which requires the state model, which requires the graph model. Read them in order if you're new to the architecture.

---

## The Core Insight

The original architecture used an "8-layer stack" framing. This was wrong. Four of the supposed layers are actually cross-cutting spines that thread through all core models rather than sitting at a specific layer.

**Core models answer**: "What is the system?"
- The anatomy, the physiology, the ecology, the senses, the interface

**Spines answer**: "How does the system operate across all models?"
- The knowledge, the memory, the engineering, the learning

Forcing the spines into the layer stack created false linearity — it implied Dimensions were a "foundation layer" and Implementation was a "top layer," when in reality both thread through every core model simultaneously.
