# Implementation Model: How to Build the Machine

> **"Now that we know what the graph is (anatomy), how it moves (physiology), how initiatives coexist (ecology), how it senses reality (the senses), and how humans and AI interact with it (the interface) — how do we actually build it?"**

This document defines the engineering of the system that runs Layers 2–6. It is intentionally focused: boundary concerns belong in [BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md), interaction concerns belong in [INTERACTION_MODEL.md](./INTERACTION_MODEL.md).

This is **Layer 7** in the 8-layer architecture stack. Metaphor: **The engineering — how the system is built.**

---

## Guiding Principles

### Principle 1: Start Manual, Earn Automation

```
Day 1:    Graph tracked in documents/issues
          Edges fire manually
          Humans do everything (T1-T2)

Month 3:  AI starts proposing transitions
          Some edges AI-assisted
          First trust promotions based on evidence

Month 6:  AI operates routine nodes/edges at T3
          Humans handle exceptions
          PULSE cadences partially automated

Year 1:   High-trust subgraphs at T4-T5
          Low-trust nodes stay at T2
          Portfolio advisor pattern active
```

Implementation must support every trust level simultaneously. A team new to a node starts at T1. A mature team with a demonstrated track record moves to T3+. These coexist in the same system.

### Principle 2: Map to Existing Tools First

Don't build a custom platform before mapping to what you have.

| Graph Concept | Tool Mapping |
|---------------|-------------|
| Node tracking | GitHub Issues (one issue per node instance) |
| Portfolio & initiative views | GitHub Projects (board per initiative, portfolio board) |
| S4 Make / L4 Generation | GitHub PRs |
| PULSE cadences, automated CHECKS | GitHub Actions (cron jobs, PR checks) |
| Maps, outcomes, bets, specs | Markdown files in repository |
| AI agents | Edge operators, cascade detectors, pulse engines |

The graph doesn't require a graph database. It requires **structured use of existing tools**.

### Principle 3: Make the Invisible Visible

The system's primary value is surfacing what was previously invisible:

- TENSIONS nobody is managing → tension/decision queue
- DEGRADED edges → signal quality alerts
- OVERLOADED singletons → resource warnings
- Suppressed RIPPLES → DANGER STATE alert
- Cross-initiative CONFLICTS → collision detection

If the system isn't surfacing these, it isn't working.

### Principle 4: The Graph Is the API

For AI agents, the graph model IS the interface contract. An AI agent's context is:

```
{
  node: "S3 Spec",
  state: "DRAFT",
  initiative: "Alpha",
  trust_level: "T2",
  incoming_feeds: ["L2 Decomposition (ACTIVE)", "L3 Contracts (ACTIVE)"],
  outgoing_feeds: ["S4 Make (EMPTY)"],
  checks_reference: "S1 Intent",
  blast_radius: 2
}
```

This context is derivable from the graph. AI agents don't need custom briefings — they need graph access.

### Principle 5: Measure the Model, Not Just the Work

The system should track its own health, not just initiative health:

| Model Metric | What It Tells You |
|-------------|------------------|
| Cascade rule accuracy | Are ripples propagating correctly? |
| PULSE cadence adherence | Are maps being refreshed on schedule? |
| TENSION resolution time | How long do contested states wait? (ignored = DEGRADED) |
| Average FEEDS edge quality | Is signal quality across the system improving? |
| Blast radius computation accuracy | Are authority decisions being made on correct data? |

These feed into Layer 8 (Meta-Evolution) — the system learns whether its own model is correct.

---

## Graph Runtime

### Data Model

The minimal data model needed to run the graph:

```
Node {
  id:           string            // e.g., "alpha/s3-spec"
  type:         NodeType          // L1, D1, M2, S4, etc.
  initiative:   string            // "alpha" | "singleton"
  state:        NodeState         // EMPTY | DRAFT | ACTIVE | STALE | CONTESTED | FAILING | RETIRED
  trust_level:  TrustLevel        // T1 | T2 | T3 | T4 | T5
  artifact_ref: string?           // link to the artifact (issue, doc, PR)
  fitness:      float?            // 0.0–1.0, derived from L6 Measurement
  bandwidth:    float?            // N1 only, 0.0–1.0
  updated_at:   timestamp
}

Edge {
  id:           string
  type:         EdgeType          // FEEDS | CHECKS | CONSTRAINS | TENSIONS | ENABLES | MIRRORS
  source:       NodeId
  target:       NodeId
  reference:    NodeId?           // CHECKS only: the reference node
  state:        EdgeState         // INACTIVE | ACTIVE | DEGRADED | BROKEN
  encode_quality:   float?        // 0.0–1.0
  transmit_quality: float?        // 0.0–1.0
  decode_quality:   float?        // 0.0–1.0
  correction_needed: boolean
}
```

### Query Patterns

The system needs to answer these questions efficiently:

| Question | Graph Query |
|----------|-------------|
| What's blocked right now? | Nodes in BLOCKED state (HARD precondition unmet) |
| What needs human attention? | CONTESTED nodes, sorted by blast radius |
| What's the blast radius of X? | Traverse forward along FEEDS edges from X, count nodes |
| Which edges are degraded? | Edges where any quality attribute < threshold |
| What would cascade if Y goes stale? | Traverse FEEDS backward from Y to find dependent nodes |
| Is the portfolio healthy? | Singleton utilization + cross-initiative edge states |

### Event Processing

State changes propagate through an event system:

```
STATE_CHANGE_EVENT {
  node: NodeId
  from_state: NodeState
  to_state: NodeState
  trigger: string        // what caused the change
  timestamp: timestamp
}
```

On receiving a STATE_CHANGE_EVENT:
1. Compute blast radius (traverse forward along FEEDS)
2. Determine authority required (blast_radius × reversibility matrix)
3. If AI-authorized: execute cascade immediately
4. If human-required: enqueue to decision queue, block downstream FLOW

### Consistency Model

Conflicting state changes can occur (two nodes transitioning simultaneously in ways that affect each other). The rule:

- TENSIONS resolution is always serialized (one at a time, human-gated)
- FLOW transitions within an initiative are optimistic (allow parallel, detect conflicts)
- Cross-initiative cascades require Portfolio Manager approval before propagating

---

## Node Embodiment

Each node needs a **home** (where it lives), a **schema** (what it contains), and a **state indicator** (visible current state).

| Node | Home | Schema | State Indicator |
|------|------|--------|-----------------|
| L1 Outcome | GitHub Issue with outcome template | outcome statement, success criteria, validated-by | Issue label: EMPTY/DRAFT/ACTIVE |
| D1 Situation Assessment | GitHub Issue with assessment template | situation type, signals observed, frame recommendation | Issue label |
| D2 Frame | Sub-section of D1 issue or linked issue | frame choice (experiment/build/complex/complicated), rationale | Issue label |
| D3 Bet | Markdown file with bet template | hypothesis, predictions, success criteria, fitness score | File + Issue label |
| S1 Intent | GitHub Issue linked to D3 Bet | intent statement, scope, excluded scope | Issue label |
| L2 Decomposition | GitHub Issue comment or child issues | work unit list, constraints applied | Issue with child issues |
| S2 Shape | GitHub Issue with shape template | boundaries, interface points, excluded scope | Issue label |
| L3 Contracts | Markdown file or GitHub Issue | interface definitions, versions, owners | File + label |
| S3 Spec | PR description or design doc | specification, acceptance criteria, technical approach | PR status |
| S4 Make | The PR itself | implementation, test coverage | PR status |
| L4 Generation | AI-generated code in PR | generated artifacts, generation context | PR review status |
| L6 Measurement | GitHub Issue with metrics template | outcome KPIs, quality signals, fitness derived | Issue with metrics |
| M1 Value Map | Markdown file (auto-updated) | value positions, movement signals | File staleness indicator |
| M2 Territory Map | Markdown file (auto-generated from analysis) | code health, deployment state, debt signals | File staleness + sensor freshness |
| M3 Context Map | Markdown file | domain relationships, contracts, team boundaries | File staleness |
| M4 Journey Map | Markdown file | user journey, drop-off points, satisfaction signals | File staleness |
| A1 Focus | GitHub Project + markdown | priority allocations, attention budget | Project board state |
| N1 Topology | Markdown file | team structure, bandwidth, assignments | File + utilization indicator |

---

## Edge Embodiment

### Within-Initiative Edge Types

| Edge Type | Practical Mechanism | Automation Level |
|-----------|--------------------|-----------------| 
| **FEEDS** | Linked GitHub Issues, PR references, explicit "depends on" | T2: AI drafts link, human confirms; T3+: AI fires automatically when source ACTIVE |
| **CHECKS** | GitHub Actions PR checks, automated test suites, review checklists | T2: checklist items; T3+: automated check with human exception handling |
| **CONSTRAINS** | Issue labels, project board rules, PR templates with constraint fields | T1-T2: manual enforcement; T3+: automated blocking |
| **TENSIONS** | Dedicated tension tracking (issue type or label), decision queue | Always human-gated for resolution; AI detects and routes |
| **ENABLES** | Documentation links, capability prerequisites in templates | T1-T2: manual verification; T3+: automated prerequisite checking |
| **MIRRORS** | Scheduled comparison jobs (GitHub Actions), staleness detection | T2: AI reports divergence; T3+: AI triggers refresh cascade |

### Cross-Initiative Edge Types

| Edge Type | Practical Mechanism |
|-----------|---------------------|
| **COMPETES** | Portfolio board cross-initiative dependency tracking |
| **CONFLICTS** | Automated detection via shared resource analysis + human review |
| **AMPLIFIES** | Portfolio board relationship documentation |
| **DEPENDS** | Explicit dependency tracking between initiative issues |

### Edge Quality Measurement

FEEDS edges carry quality attributes (encode/transmit/decode quality). Measurement approaches:

- **encode_quality**: Measured by clarity scores on source artifacts, or by downstream confusion signals (rework rate, clarification requests)
- **transmit_quality**: Measured by latency and completeness of information transfer (was the artifact shared? was context included?)
- **decode_quality**: Measured by alignment between sender's intent and receiver's interpretation (explicit verification steps, retrospective analysis)

---

## Event System

The three movement types from [STATE_MODEL.md](./STATE_MODEL.md) each require different infrastructure:

### FLOW (pull-based)

```
Mechanism: Issue/PR state transitions
When: Node completes (DRAFT → ACTIVE), downstream nodes unlock
Implementation: GitHub Actions triggered by issue label changes
               or PR merge events
Pattern: Downstream node "pulls" — initiates when it's ready
```

### RIPPLE (push-based)

```
Mechanism: Webhook-triggered cascade processing
When: State change detected that should propagate outward
Implementation: GitHub Actions workflow triggered by state change events
               Traverse affected edges, queue updates
Pattern: Upstream node "pushes" — change radiates outward immediately
```

### PULSE (time-triggered)

```
Mechanism: Scheduled GitHub Actions workflows
When: Time-based schedule (daily/weekly/monthly per cadence)
Implementation: Cron-triggered workflows that:
               1. Check sensor freshness (boundary model)
               2. Refresh stale maps
               3. Run CHECKS on active nodes
               4. Generate health reports
               5. Trigger RIPPLES if drift detected
Pattern: Clock drives — independent of node activity
```

---

## Integration Architecture

How implementation connects to the other models:

```
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 7: IMPLEMENTATION                                        │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ Graph Runtime│  │Event System  │  │ Node/Edge Embodiment │  │
│  │ (data model) │  │(FLOW/RIPPLE/ │  │ (GitHub Issues/PRs/  │  │
│  │              │  │ PULSE)       │  │  Actions/Markdown)   │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘  │
│         │                 │                      │              │
└─────────┼─────────────────┼──────────────────────┼─────────────┘
          │                 │                      │
          ▼                 ▼                      ▼
┌──────────────────┐  ┌─────────────┐  ┌────────────────────────┐
│  LAYER 5:        │  │  LAYER 6:   │  │  LAYERS 2-4:           │
│  BOUNDARY MODEL  │  │  INTERACTION│  │  GRAPH / STATE /       │
│                  │  │  MODEL      │  │  PORTFOLIO MODELS      │
│  Sensors:        │  │             │  │                        │
│  - Connect to    │  │  Views:     │  │  Rules implemented:    │
│    external data │  │  - My Work  │  │  - State machines      │
│  Actuators:      │  │  - Tension  │  │  - Cascade rules       │
│  - Push changes  │  │  - Signal   │  │  - Authority matrix    │
│    to real world │  │  - etc.     │  │  - Preconditions       │
└──────────────────┘  └─────────────┘  └────────────────────────┘
```

**API contract between layers**:
- Implementation reads sensor data from boundary model → updates map nodes
- Implementation exposes views from interaction model → reads graph state, renders
- Implementation enforces rules from graph/state/portfolio models → state machines, cascade logic, precondition checking

---

## Full Architecture Stack

| Layer | Model | Metaphor | Document |
|-------|-------|----------|----------|
| 8 | Meta-Evolution | The evolution | (future) |
| **7** | **Implementation (this document)** | **The engineering** | **IMPLEMENTATION_MODEL.md** |
| 6 | Interaction | The interface | [INTERACTION_MODEL.md](./INTERACTION_MODEL.md) |
| 5 | Boundary | The senses | [BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md) |
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
| [BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md) | **The senses** — how it perceives and affects reality | Sensors, actuators, feedback loops, boundary health |
| [INTERACTION_MODEL.md](./INTERACTION_MODEL.md) | **The interface** — how participants engage | Roles, views, AI patterns, trust-graduated collaboration |
| **IMPLEMENTATION_MODEL.md** (this document) | **The engineering** — how it's built | Graph runtime, node/edge embodiment, event system, integration |

The implementation model is the engineering layer. It takes everything defined in Layers 2–6 and answers the question: **how does this actually run?** It maps abstract graph concepts to concrete tools, and defines the plumbing that makes the whole machine work.
