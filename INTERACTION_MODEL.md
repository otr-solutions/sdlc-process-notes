# Interaction Model: How Humans and AI Participate in the Graph

> **"The graph model IS the interface contract for AI agents."**

This document defines who interacts with the system and how — what they see, what decisions they make, and how trust graduation changes the collaboration over time.

This is **Layer 6** in the 8-layer architecture stack. Metaphor: **The interface — how participants engage with the system.**

The boundary model ([BOUNDARY_MODEL.md](./BOUNDARY_MODEL.md)) defines sensors and actuators — the physical boundary. This model defines the *human and AI* interaction surface on top of that boundary.

---

## Roles and Their Relationship to the Graph

| Role | Graph Relationship | Key Decisions |
|------|-------------------|---------------|
| **Contributor** | Operates within nodes (writes code, specs, designs) | Node-level: "Is this artifact good enough?" |
| **Initiative Lead** | Manages one initiative's instance graph | Initiative-level: "What's the right bet? Right shape?" |
| **Portfolio Manager** | Manages cross-initiative dynamics at singleton layer | Portfolio-level: PRIORITIZE, SEQUENCE, RESOLVE, KILL, SPAWN |
| **Architect** | Manages signal quality, domain boundaries, contracts | Cross-cutting: "Are edges healthy? Are boundaries right?" |
| **AI Agent** | Operates nodes, edges, sensors, and actuators per trust level | Varies by trust level and pattern |

### Role → Graph Mapping

| Role | Nodes Touched | Edges Touched | Decisions |
|------|---------------|---------------|-----------|
| Contributor | S3 Spec, S4 Make, L4 Generation | FEEDS into/out of their nodes | DRAFT → ACTIVE on their artifacts |
| Initiative Lead | D1–D3, S1–S4, L1–L3, L6 | All within-initiative edges | TENSIONS resolution, bet framing, shape decisions |
| Portfolio Manager | A1 Focus, N1 Topology (singletons), portfolio states | COMPETES, CONFLICTS, AMPLIFIES, DEPENDS, KILL | PRIORITIZE, SEQUENCE, KILL, SPAWN |
| Architect | M2–M3, L3 Contracts, N1 Topology | CHECKS edges, MIRRORS edges | Signal degradation, domain boundary decisions |
| AI Agent | Per trust level — any node/edge at appropriate autonomy | Per trust level | Per trust level |

---

## Human Views — What People Need to See

Five views derived from the graph, one per primary role.

### My Work View (for Contributors)

```
┌─────────────────────────────────────────────────────┐
│  MY WORK                                            │
│                                                     │
│  Nodes I own:                                       │
│    S3 Spec [DRAFT] — awaiting my review             │
│    S4 Make [ACTIVE] — in progress                   │
│                                                     │
│  Edges waiting on me:                               │
│    S3 Spec ──FEEDS──▶ S4 Make [INACTIVE — unblock?] │
│                                                     │
│  Decisions for my input:                            │
│    S3 Spec [CONTESTED] — shape conflict needs input │
└─────────────────────────────────────────────────────┘
```

Like a personalized task list derived from the graph — not a manually maintained to-do list, but a live view of what the graph says this person needs to do.

### Initiative View (for Initiative Leads)

```
┌─────────────────────────────────────────────────────┐
│  INITIATIVE: [Name]                                 │
│                                                     │
│  Graph state (12 instance nodes):                   │
│    D1 [ACTIVE] D2 [ACTIVE] D3 [ACTIVE]             │
│    S1 [ACTIVE] S2 [ACTIVE] L1 [ACTIVE]             │
│    L2 [ACTIVE] L3 [ACTIVE] S3 [DRAFT]              │
│    S4 [EMPTY]  L4 [EMPTY]  L6 [EMPTY]              │
│                                                     │
│  Active tensions:       2 unresolved                │
│  CONTESTED states:      1 (S3 Spec)                 │
│  CHECKS passing:        4/5                         │
│  FLOW progress:         Definition → Shaping ✓      │
│  FEEDS edge quality:    avg 0.82                    │
└─────────────────────────────────────────────────────┘
```

### Portfolio View (for Portfolio Managers)

```
┌─────────────────────────────────────────────────────┐
│  PORTFOLIO                                          │
│                                                     │
│  Initiatives:                                       │
│    Alpha  [ACTIVE]   — 3 nodes CONTESTED           │
│    Beta   [ACTIVE]   — healthy                      │
│    Gamma  [QUEUED]   — waiting on A1 Focus          │
│    Delta  [PROPOSED] — needs triage                 │
│                                                     │
│  Cross-initiative edges:                            │
│    Alpha ──COMPETES──▶ Beta (Team Onboarding)       │
│    Gamma ──DEPENDS──▶  Alpha (L3 Contracts)         │
│                                                     │
│  Singleton utilization:                             │
│    A1 Focus:    STRETCHED (87% allocated)           │
│    N1 Topology: BALANCED  (62% allocated)           │
│                                                     │
│  Portfolio health: STRETCHED                        │
└─────────────────────────────────────────────────────┘
```

### Tension View (for Decision Makers)

```
┌─────────────────────────────────────────────────────┐
│  DECISION QUEUE                                     │
│                                                     │
│  1. [HIGH] Alpha: D3 Bet ◀──TENSIONS──▶ M2 Territory│
│     Blast radius: 6 nodes if unresolved             │
│     Blocking: S1 Intent, S2 Shape, S3 Spec         │
│     Wait time: 3 days                               │
│                                                     │
│  2. [MED]  Beta: A1 Focus ◀──TENSIONS──▶ L2 Decomp  │
│     Blast radius: 2 nodes                           │
│     Blocking: S3 Spec                               │
│     Wait time: 1 day                                │
└─────────────────────────────────────────────────────┘
```

Sorted by blast radius (highest impact first). This is the "human judgment queue" — the things that AI cannot resolve and need human attention now.

### Signal View (for Architects)

```
┌─────────────────────────────────────────────────────┐
│  SIGNAL HEALTH                                      │
│                                                     │
│  DEGRADED edges (3):                                │
│    Alpha: D3 Bet ──FEEDS──▶ S1 Intent               │
│      encode_quality: 0.4 ← below threshold         │
│    Beta: L3 Contracts ──FEEDS──▶ S3 Spec            │
│      decode_quality: 0.5 ← below threshold         │
│    Gamma: M3 Context ──FEEDS──▶ N1 Topology         │
│      transmit_quality: 0.3 ← critical              │
│                                                     │
│  CHECKS failures (1):                               │
│    Alpha: S4 Make vs L1 Outcome — FAILING           │
│                                                     │
│  Sensor freshness:                                  │
│    M2 Territory: STALE (last updated 8 days ago)    │
│    M4 Journey:   CURRENT                            │
└─────────────────────────────────────────────────────┘
```

A system health dashboard for information flow — not business outcomes, but the quality of the signals flowing through the graph.

---

## AI Interaction Patterns

Five patterns for how AI participates in the graph.

### Pattern 1: AI as Node Operator

AI works WITHIN a node — generating artifacts, drafting content, processing inputs.

```
Example: L4 Generation
  Input:  S3 Spec [ACTIVE] ──FEEDS──▶ L4 Generation
  AI act: Generate code from spec
  Output: Code artifact → S4 Make [DRAFT]
  
At T2: AI drafts, human reviews everything before DRAFT → ACTIVE
At T4: AI operates the node, human monitors; exceptions escalated
```

Trust governs how much autonomy AI has within the node. The node's trust_level attribute determines the human/AI split.

### Pattern 2: AI as Edge Operator

AI works ON edges — encoding, transmitting, decoding information between nodes.

```
Example: FEEDS edge from D3 Bet → S1 Intent
  AI act: Translate bet statement into intent statement
          (encoding: "what does this bet mean as an intent?")
  
At T2: AI proposes the encoding, human fires the edge
At T3: AI fires routine edges, human handles complex translations
```

Trust governs whether AI fires the edge autonomously or proposes it for human approval.

### Pattern 3: AI as Cascade Detector

AI monitors the graph for state changes that should trigger cascades.

```
Example: Detecting M2 Territory drift via MIRRORS edge
  AI monitors: CODE_HEALTH sensor readings vs M2 Territory map
  Detects: Reality has diverged from map (STALE threshold crossed)
  
At T2: AI flags potential cascade, human evaluates and triggers
At T3: AI triggers routine cascades, human handles high-blast-radius
```

### Pattern 4: AI as Portfolio Advisor

AI analyzes cross-initiative relationships and proposes portfolio decisions.

```
Example: Competing initiative analysis
  AI detects: Initiative A and B compete for same team capacity
  AI analyzes: Value/fitness scores across both
  AI proposes: "Prioritize A based on value/fitness; sequence B for next cycle"
  
At T2: AI provides data summaries, humans decide
At T3: AI proposes decisions, humans approve
At T4: AI makes tactical portfolio decisions, humans handle strategic
```

### Pattern 5: AI as Pulse Engine

AI runs scheduled PULSE operations — refreshing maps, scanning for drift, evaluating health.

```
Example: Weekly resource pulse
  AI scans: N1 Topology allocations across all initiatives
  AI flags: Imbalances, overloaded teams, underutilized capacity
  AI generates: Resource health report
  
At T2: AI generates reports, humans review
At T3: AI runs routine pulses, flags anomalies for human review
At T4: AI runs all pulses, humans review monthly
```

---

## Trust-Graduated Collaboration

How human/AI interaction changes at each trust level. Trust levels apply to EDGES, not just nodes — different nodes and edges in the same initiative may be at different automation levels.

| Trust Level | Node Operation | Edge Operation | Cascade Detection | Portfolio Advisory | Pulse Engine |
|------------|---------------|---------------|-------------------|-------------------|-------------|
| **T1 Human Only** | Human does everything | Human fires all edges manually | Human detects | Human decides | Human runs reviews |
| **T2 AI Drafts** | AI drafts artifacts, human reviews all | AI proposes edge content, human fires | AI flags potential cascades, human evaluates | AI provides data summaries | AI generates reports, human reviews |
| **T3 AI Decides** | AI produces artifacts, human reviews exceptions | AI fires routine edges, human handles complex | AI triggers routine cascades, human handles high-blast | AI proposes decisions, human approves | AI runs routine pulses, flags anomalies |
| **T4 AI Autonomous** | AI operates nodes, human monitors | AI fires edges autonomously, human audits | AI manages cascades, human reviews periodically | AI makes tactical decisions, human handles strategic | AI runs all pulses, human reviews monthly |
| **T5 Full Autonomy** | AI fully autonomous | AI fully autonomous | AI fully autonomous | AI fully autonomous within guardrails | AI fully autonomous |

**Key insight**: The system starts at T1 (well-organized manual process) and gradually automates edges as trust is earned. Moving from T2 to T3 requires evidence of consistent AI performance at T2. Trust can also be *demoted* when CHECKS edges reveal quality problems.

**Invariant**: CONTESTED resolution is always human, regardless of trust level. Tensions are by definition where human judgment is irreplaceable. See [STATE_MODEL.md](./STATE_MODEL.md) for the authority model.

---

## Decision Routing

Decisions are detected, routed, and resolved through the graph.

**Detection**:
- CONTESTED states (TENSIONS edge activates and nodes disagree)
- CHECKS failures (output doesn't match reference)
- TENSIONS activation (opposing forces both go ACTIVE)

**Routing** is based on decision type and blast radius (computed from graph traversal — see [STATE_MODEL.md](./STATE_MODEL.md)):

| Decision Type | Low Blast Radius | High Blast Radius |
|---------------|-----------------|------------------|
| Node transition | AI acts (T3+), notifies | AI proposes, human approves |
| TENSIONS resolution | Human always | Human always |
| Portfolio change | Initiative Lead | Portfolio Manager |
| KILL decision | Human always | Human always |

**Recording**: Decision outcomes are written back into the graph as state transitions — the graph maintains history of what was decided and why.

**Decision latency**: Unresolved CONTESTED states block FLOW along all downstream FEEDS edges. Every day a tension sits unresolved is a day the downstream nodes can't progress. The Tension View exists to surface this cost.

---

## The Graph as AI's API

The graph model IS the interface contract for AI agents. An AI agent doesn't need to understand the entire system — it needs:

```
AGENT CONTEXT (minimal interface):
  - What node am I on? → node identity + artifact type
  - What state? → current state (EMPTY/DRAFT/ACTIVE/etc.)
  - What edges connect? → incoming FEEDS (my inputs) + outgoing (my outputs)
  - What's my trust level? → T1-T5 → what I'm allowed to do
  - What's the cascade impact? → blast radius of my actions

AGENT OPERATION:
  - Read: incoming FEEDS edges → consume upstream artifacts
  - Write: produce artifact → update node state → fire outgoing edges
  - Flag: CONTESTED/FAILING states I detect → route to human
  - Check: CHECKS edges against reference → report quality signal
```

This means the graph structure directly determines the prompt context for AI agents. The graph isn't just documentation — it's a machine-readable specification of what each AI agent should do, when, and with what authority.

---

## Full Architecture Stack

| Layer | Model | Metaphor | Document |
|-------|-------|----------|----------|
| 8 | Meta-Evolution | The evolution | (future) |
| 7 | Implementation | The engineering | [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) |
| **6** | **Interaction (this document)** | **The interface** | **INTERACTION_MODEL.md** |
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
| **INTERACTION_MODEL.md** (this document) | **The interface** — how participants engage | Roles, views, AI patterns, trust-graduated collaboration |
| [IMPLEMENTATION_MODEL.md](./IMPLEMENTATION_MODEL.md) | **The engineering** — how it's built | Graph runtime, node/edge embodiment, event system |

The interaction model sits between the boundary (what the system can sense/affect) and the implementation (how to build it). It defines the *human and AI experience* of working with the graph — the views, the decisions, the patterns of collaboration.
