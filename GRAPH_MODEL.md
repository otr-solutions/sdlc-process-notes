# Graph Model: Integrating the Nine Dimensions

> **"When a real piece of work enters the system, which dimensions activate, in what order, and who (human or AI) does what at each step?"**

## Why a Graph?

The nine dimensions (v1 through v9) each describe a facet of how software moves from idea to delivery and back again. Early attempts to organize them into sequential phases or nested loops broke down — individual steps within each dimension operate at different tempos and serve different roles. Some are persistent artifacts, some are actions that move information, and some are properties that configure behavior.

A **graph model** — nodes, edges, and attributes — captures the real structure without forcing false linearity.

---

## Nodes (18)

A node is something that **has persistent state, produces artifacts, or requires a decision**. If something purely moves information between other things, it's an edge, not a node.

### Derivation from Dimensions

Each of the 42 original steps across all 9 dimensions was tested against three criteria:
1. Does it have persistent state? (defined/undefined, current/stale)
2. Does it produce an artifact? (a document, map, decision, measurement)
3. Does it require a standalone decision? (not just executing another node's output)

Steps that failed all three became edges, attributes, or operations instead.

### Definition Nodes — "What are we doing?"

| Node | Source | State | Artifact | Key Question |
|------|--------|-------|----------|--------------|
| **L1 Outcome Definition** | v1-layered | defined / undefined / validated | The outcome statement | "What do we want to change?" |
| **D1 Situation Assessment** | v4-decision | sensing / sensed | Situation assessment | "What kind of problem is this?" |
| **D2 Frame** | v4-decision | framed / not | The frame (experiment vs build, complex vs complicated) | "How do we approach this?" |
| **D3 Bet** | v4-decision | bet made / not | Bet statement + assumptions | "What do we believe will work?" |
| **S1 Intent** | v2-flow | captured / not | Intent statement | "What specifically are we doing?" |

### Shaping Nodes — "How do we structure the work?"

| Node | Source | State | Artifact | Key Question |
|------|--------|-------|----------|--------------|
| **L2 Decomposition** | v1-layered | decomposed / not | Work units | "What are the pieces?" |
| **S2 Shape** | v2-flow | shaped / not | Work boundaries | "What are the boundaries?" |
| **L3 Contracts** | v1-layered | defined / agreed / broken | Interface contracts | "What are the interfaces?" |
| **S3 Spec** | v2-flow | specified / not | The specification | "What exactly gets built?" |

### Execution Nodes — "Doing the work"

| Node | Source | State | Artifact | Key Question |
|------|--------|-------|----------|--------------|
| **S4 Make** | v2-flow | building / done | The built thing | "Build it" |
| **L4 Generation** | v1-layered | generating / complete | Code and artifacts | "AI/human produces artifacts" |

### Measurement Nodes — "Did it work?"

| Node | Source | State | Artifact | Key Question |
|------|--------|-------|----------|--------------|
| **L6 Measurement** | v1-layered | measured / not | Metrics, outcome data | "What actually happened?" |

### Map Nodes — "Persistent shared awareness"

| Node | Source | State | Artifact | Key Question |
|------|--------|-------|----------|--------------|
| **M1 Value Map** | v8-map | current / stale | Value map (Wardley-inspired) | "Where is value created?" |
| **M2 Territory Map** | v8-map | current / stale | Territory map (ground truth) | "What exists right now?" |
| **M3 Context Map** | v8-map | current / stale | Context map (DDD-inspired) | "How do domains relate?" |
| **M4 Journey Map** | v8-map | current / stale | Journey map (user flow) | "How does value flow?" |

### Resource Nodes — "What's available?"

| Node | Source | State | Artifact | Key Question |
|------|--------|-------|----------|--------------|
| **A1 Focus** | v3-attention | focused on X / available | Priority decisions | "Where are humans spending attention?" |
| **N1 Topology** | v6-network | defined / undefined | Team structure map | "Who works on what?" |

---

## Edges (6 Types)

Edges describe **how nodes relate to each other**. Each type represents a fundamentally different kind of relationship.

### FEEDS (A → B) — Information Flow

One node's output becomes another node's input. Directional.

```
M2 Territory ──FEEDS──▶ L1 Outcome         "Reality informs what we try to change"
M1 Value     ──FEEDS──▶ D2 Frame            "Value position informs approach"
M4 Journey   ──FEEDS──▶ A1 Focus            "User bottleneck informs where humans look"
M3 Context   ──FEEDS──▶ N1 Topology         "Domain relationships inform team structure"
D1 Sense     ──FEEDS──▶ D2 Frame            "Situation assessment informs framing"
D2 Frame     ──FEEDS──▶ D3 Bet              "Frame informs what we bet on"
D3 Bet       ──FEEDS──▶ S1 Intent           "Bet becomes the intent"
L1 Outcome   ──FEEDS──▶ L2 Decomposition    "Outcome gets broken down"
S1 Intent    ──FEEDS──▶ S2 Shape            "Intent gets shaped"
S2 Shape     ──FEEDS──▶ L3 Contracts        "Shape defines boundaries"
L2 Decomp    ──FEEDS──▶ S3 Spec             "Work units get specified"
L3 Contracts ──FEEDS──▶ S3 Spec             "Contracts constrain the spec"
S3 Spec      ──FEEDS──▶ S4 Make / L4 Gen    "Spec drives building"
L6 Measure   ──FEEDS──▶ M2 Territory        "Results update reality map"
L6 Measure   ──FEEDS──▶ M1 Value            "Results shift value positions"
L6 Measure   ──FEEDS──▶ D1 Sense            "Results inform next sensing"
L6 Measure   ──FEEDS──▶ A1 Focus            "Results release/redirect attention"
```

### CHECKS (A → B against C) — Validation

A validates B against reference C. Triangular relationship that produces a pass/fail/drift signal.

```
L6 Measure ──CHECKS──▶ S4 Make    against    L1 Outcome
   "Did what we built achieve the outcome we defined?"

S3 Spec    ──CHECKS──▶ S4 Make    against    S1 Intent
   "Does what we built match the spec which should reflect intent?"

D1 Sense   ──CHECKS──▶ M2 Territory    against    reality
   "Does our territory map still match the actual territory?"

M4 Journey ──CHECKS──▶ L6 Measure    against    user experience
   "Is the user journey we mapped matching real user behavior?"
```

Every CHECKS edge carries signal fidelity attributes (from v7-signal):
- **Encode quality**: How well was the reference (C) articulated?
- **Transmit quality**: Did the reference survive the handoff?
- **Decode quality**: Was it correctly interpreted on receipt?
- **Correction needed?**: If check fails, what's the gap?

### CONSTRAINS (A → B) — Boundary Setting

A doesn't give B content — it gives B **boundaries**. B operates freely within those boundaries.

```
A1 Focus     ──CONSTRAINS──▶ L2 Decomposition
   "We can only focus on X things, so decompose accordingly"

D2 Frame     ──CONSTRAINS──▶ S2 Shape
   "This is an experiment, not a build — shape it small"

L3 Contracts ──CONSTRAINS──▶ L4 Generation
   "Generate within these interface boundaries"

N1 Topology  ──CONSTRAINS──▶ L3 Contracts
   "Team boundaries constrain where contracts must exist"

M3 Context   ──CONSTRAINS──▶ L2 Decomposition
   "Domain boundaries constrain how you can decompose"
```

**Note on BLOCKED state**: When a CONSTRAINS edge is ACTIVE and the constraining node's precondition is not yet met, the target node enters a BLOCKED state — it cannot transition until the constraint source is satisfied. See [STATE_MODEL.md](./STATE_MODEL.md) for full precondition and BLOCKED state details.

### TENSIONS (A ↔ B) — Opposing Forces

Bidirectional. Neither node is "right" — the system must **negotiate** between them. These are where human judgment is most critical.

```
A1 Focus     ◀──TENSIONS──▶ L2 Decomposition
   "Focus wants fewer things. Decomposition might create many."

D3 Bet       ◀──TENSIONS──▶ M2 Territory
   "Bets want to change reality. Territory shows constraints."

S3 Spec      ◀──TENSIONS──▶ S2 Shape
   "Spec wants precision. Shape wants to keep options open."

L1 Outcome   ◀──TENSIONS──▶ N1 Topology
   "Outcome wants the right solution. Topology shows who's available."

D2 Frame     ◀──TENSIONS──▶ A1 Focus
   "Framing as experiment wants exploration. Focus wants concentration."

M1 Value     ◀──TENSIONS──▶ D3 Bet
   "Value map shows where investment should go. Bet might disagree."
```

### ENABLES (A → B or A → edge) — Creates Conditions

A doesn't give B information or constraints — A creates the **channel or capability** that B needs to exist. Note: ENABLES can target other edges, not just nodes.

```
N1 Topology  ──ENABLES──▶ all FEEDS edges that cross team boundaries
   "Team structure creates the channels through which information flows"

M3 Context   ──ENABLES──▶ L3 Contracts
   "Knowing domain relationships makes it possible to define contracts"

M2 Territory ──ENABLES──▶ D1 Sense
   "Having a reality map makes it possible to sense changes"

A1 Focus     ──ENABLES──▶ CHECKS edges
   "You can only check what you're paying attention to"
```

### MIRRORS (A ↔ B) — Same Pattern, Different Scale

Bidirectional resonance. When one changes, the other should probably be re-examined. Neither feeds the other — they reflect the same underlying reality at different granularities.

```
D1 Sense     ◀──MIRRORS──▶ M2 Territory
   "Per-decision sensing ↔ System-wide territory awareness"

L1 Outcome   ◀──MIRRORS──▶ M4 Journey
   "Initiative outcome ↔ User journey outcome"

L3 Contracts ◀──MIRRORS──▶ M3 Context
   "Per-unit interfaces ↔ System-wide domain relationships"

N1 Topology  ◀──MIRRORS──▶ M3 Context
   "Team structure ↔ Domain structure (Conway's Law)"
```

---

## Node Attributes

Three concepts from the dimensions turned out to be neither nodes nor edges — they're **properties** that change how nodes and edges behave.

### Trust Level (from v5-trust: T1–T5)

An attribute on any node involving AI participation. Determines how that node operates.

```
T1  Human Only     — No AI involvement
T2  AI Drafts      — AI produces, human reviews everything
T3  AI Decides     — AI makes routine decisions, human handles exceptions
T4  AI Autonomous  — AI operates independently, human monitors
T5  Full Autonomy  — AI operates without human oversight
```

Applied to nodes like:
```
Node: L4 Generation
├── trust_level: T2 (AI Drafts, Human Reviews)
├── This means: AI produces, human validates
└── Changes the behavior of all CHECKS edges FROM this node
```

### Bandwidth (from v6-network: N3)

An attribute on N1 Topology and by extension on teams. Determines capacity constraints.

```
Node: N1 Topology
├── teams:
│   ├── Onboarding Team: { bandwidth: 0.6, focus: [S3, S4] }
│   └── Platform Team: { bandwidth: 0.3, focus: [L3] }
└── Bandwidth constrains which FEEDS edges can actually fire
```

### Fitness (from v9-evolutionary: E3)

A computed attribute on any node that has gone through L6 Measurement. Derived from comparing results to predictions.

```
Node: D3 Bet
├── fitness: 0.7 (bet was 70% right)
└── Derived from: L6 Measurement CHECKS D3 against L1
```

---

## Edge Attributes (from v7-signal)

Every edge in the graph — especially FEEDS and CHECKS edges — carries signal fidelity properties:

| Attribute | What It Measures | Source |
|-----------|-----------------|--------|
| `encode_quality` | How well was information prepared for sending? | P2 Encode |
| `transmit_quality` | Did information survive the transfer? | P3 Transmit |
| `decode_quality` | Was information correctly interpreted on receipt? | P1 Decode |
| `correction_needed` | Boolean — triggers P4 Correct behavior | P4 Correct |

These attributes make v7-signal a **quality layer on top of all edges** rather than a parallel dimension.

---

## Edge Operations (12)

These are the original "steps" that turned out to be **behaviors that act on the graph** rather than standalone nodes. They fire edges, update attributes, repair connections, branch paths, and select survivors.

### Signal Operations (from v7-signal)
| Operation | Acts On | What It Does |
|-----------|---------|-------------|
| **P1 Decode** | Receiving end of FEEDS edges | Interprets incoming information |
| **P2 Encode** | Sending end of FEEDS edges | Prepares information for transmission |
| **P3 Transmit** | FEEDS edges in motion | Moves information between nodes |
| **P4 Correct** | Failed CHECKS edges | Repairs signal degradation |

### Network Operations (from v6-network)
| Operation | Acts On | What It Does |
|-----------|---------|-------------|
| **N2 Interface** | FEEDS edges crossing team boundaries | Defines how cross-team edges work |
| **N4 Sync** | Multiple FEEDS edges between teams | Coordinates timing across team boundaries |

### Attention Operations (from v3-attention)
| Operation | Acts On | What It Does |
|-----------|---------|-------------|
| **A2 Chunk** | CONSTRAINS edges from A1 Focus | Sizes work to fit human/AI capacity |
| **A3 Offload** | trust_level attribute on nodes | Sets the human/AI split on nodes |
| **A4 Refresh** | A1 Focus node | Updates focus after L6 Measurement completes |

### Decision Operations (from v4-decision)
| Operation | Acts On | What It Does |
|-----------|---------|-------------|
| **D4 Learn** | D1, D2, D3 nodes | Updates decision nodes after L6 Measurement |

### Evolution Operations (from v9-evolutionary)
| Operation | Acts On | What It Does |
|-----------|---------|-------------|
| **E1 Vary** | Graph paths | Creates parallel branches (alternative approaches) |
| **E2 Select** | Parallel branches | Chooses which variant survives based on fitness |
| **E4 Adapt** | Nodes and attributes | Modifies the system based on selection results |

---

## Three Layers of Abstraction

The complete model has three distinct layers:

```
┌─────────────────────────────────────────────────────┐
│  LAYER 3: OPERATIONS (Runtime)                      │
│  "What happens to the graph"                        │
│                                                     │
│  12 operations that fire edges, update attributes,  │
│  repair connections, branch paths, select survivors  │
│                                                     │
│  Signal: P1 P2 P3 P4                               │
│  Network: N2 N4                                     │
│  Attention: A2 A3 A4                                │
│  Decision: D4                                       │
│  Evolution: E1 E2 E4                                │
├─────────────────────────────────────────────────────┤
│  LAYER 2: ATTRIBUTES (Configuration)                │
│  "Properties that change how nodes and edges behave"│
│                                                     │
│  Node attrs:  trust_level, bandwidth, fitness       │
│  Edge attrs:  encode_quality, transmit_quality,     │
│               decode_quality, correction_needed     │
├─────────────────────────────────────────────────────┤
│  LAYER 1: THE GRAPH (Structure)                     │
│  "What exists and how things relate"                │
│                                                     │
│  18 nodes + 6 edge types                            │
│  (FEEDS, CHECKS, CONSTRAINS, TENSIONS,              │
│   ENABLES, MIRRORS)                                 │
└─────────────────────────────────────────────────────┘
```

This maps to how systems actually work:
- **Layer 1 (Graph)** = the database schema
- **Layer 2 (Attributes)** = the data and configuration
- **Layer 3 (Operations)** = the application logic

---

## Traceability to Dimensions

Every original dimension contributes to the graph model, but not every dimension contributes the same *kind* of element:

| Dimension | Contributes Nodes | Contributes Edges | Contributes Attributes | Contributes Operations |
|-----------|:-:|:-:|:-:|:-:|
| v1-layered | L1, L2, L3, L4, L6 | — | — | — |
| v2-flow | S1, S2, S3, S4 | — | — | — |
| v3-attention | A1 | — | — | A2, A3, A4 |
| v4-decision | D1, D2, D3 | — | — | D4 |
| v5-trust | — | — | trust_level (T1-T5) | — |
| v6-network | N1 | — | bandwidth | N2, N4 |
| v7-signal | — | — | encode/transmit/decode quality, correction_needed | P1, P2, P3, P4 |
| v8-map | M1, M2, M3, M4 | — | — | — |
| v9-evolutionary | — | — | fitness | E1, E2, E4 |

Note: The 6 edge types (FEEDS, CHECKS, CONSTRAINS, TENSIONS, ENABLES, MIRRORS) emerged from analyzing relationships *between* dimensions rather than from any single dimension.

---

## What This Means for the Central Hub

If the system is a graph with 18 nodes, 6 edge types, 7 attributes, and 12 operations, then the central hub is essentially a **graph database with a visual query layer**. For any given initiative, the hub needs to surface:

1. **Which nodes are active** — not all 18 are relevant to every piece of work
2. **Which edges are firing** — what's feeding, constraining, blocking, enabling
3. **Where tensions exist** — these are the decisions that need human judgment
4. **Where mirrors are out of sync** — same work at different scales has diverged
5. **Which checks are passing or failing** — signal fidelity across the graph
6. **What operations are pending** — what needs to happen next

**TENSIONS are the most valuable thing to surface.** Everything else is plumbing — information flowing, constraints applying, sequences unfolding. But tensions are where the interesting decisions live, where trade-offs get made, and where human judgment is irreplaceable.

---

## Relationship to Other Models

This is the first of three companion documents:

| Document | Metaphor | What It Defines |
|----------|----------|----------------|
| **GRAPH_MODEL.md** (this document) | **Anatomy** — what exists | 18 nodes, 6 edge types, attributes, operations |
| [STATE_MODEL.md](./STATE_MODEL.md) | **Physiology** — how it moves | States, transitions, authority, cascades, movement types |
| [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) | **Ecology** — how multiple organisms interact | Instance vs singleton nodes, cross-initiative edges, portfolio decisions, arbitration |

The graph is the **anatomy**. The state model is the **physiology**. The portfolio model is the **ecology**.
