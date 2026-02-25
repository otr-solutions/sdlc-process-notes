# Portfolio Model: Multiple Initiatives in a Shared Reality

> **The graph model is the anatomy. The state model is the physiology. The portfolio model is the ecology — how multiple organisms interact in a shared environment.**

Everything in [GRAPH_MODEL.md](./GRAPH_MODEL.md) and [STATE_MODEL.md](./STATE_MODEL.md) describes a single initiative in isolation. But a real factory has 5, 10, 50 things in flight simultaneously, all competing for the same humans, the same AI capacity, the same attention, and the same codebase. This document defines how the model scales to that reality.

---

## The Core Problem: Shared Resources

In a single-initiative graph, every node "owns" its own state and transitions happen cleanly. When multiple initiative graphs run simultaneously, they **collide** at shared nodes:

```
INITIATIVE A: "Reduce customer churn"
  Uses: N1 Topology (Onboarding Team)
  Uses: A1 Focus (human attention budget)
  Uses: M2 Territory (codebase reality)
  Modifies: S4 Make → changes the codebase

INITIATIVE B: "Launch billing redesign"
  Uses: N1 Topology (SAME Onboarding Team + Billing Team)
  Uses: A1 Focus (SAME human attention budget)
  Uses: M2 Territory (SAME codebase reality)
  Modifies: S4 Make → changes the SAME codebase

These two initiatives don't know about each other
unless the model explicitly connects them.
```

---

## Node Classification: Instance vs Singleton

Not all 18 nodes from the graph model behave the same way when multiple initiatives exist.

### Instance Nodes (12) — One Per Initiative

Each initiative gets its own independent copy of these nodes. Initiative A's L1 Outcome is completely separate from Initiative B's L1 Outcome.

```
INSTANCE NODES:
├── L1  Outcome Definition
├── D1  Situation Assessment
├── D2  Frame
├── D3  Bet
├── S1  Intent
├── L2  Decomposition
├── S2  Shape
├── L3  Contracts
├── S3  Spec
├── S4  Make
├── L4  Generation
└── L6  Measurement
```

### Singleton Nodes (5) — Shared Across All Initiatives

There's only one territory map, one team topology, one attention budget. These are where collisions happen.

```
SINGLETON NODES:
├── M1  Value Map         — one picture of where value lives
├── M2  Territory Map     — one picture of current reality
├── M3  Context Map       — one picture of domain relationships
├── N1  Topology          — one team structure
└── A1  Focus             — one human attention budget
```

### Hybrid Nodes (1) — Singleton Master + Initiative Views

```
HYBRID NODES:
└── M4  Journey Map       — one master user journey, but each initiative
                            maps a subset of that journey. When Initiative A
                            modifies step 3, Initiative B needs to know if
                            it also touches step 3.
```

---

## The Arena: Multi-Initiative Graph Structure

```
┌─────────────────────────────────────────────────────────────┐
│                      THE ARENA                               │
│                                                              │
│  SINGLETON LAYER (shared reality)                            │
│  ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐         │
│  │ M1  │ │ M2  │ │ M3  │ │ M4  │ │ A1  │ │ N1  │         │
│  │Value│ │Terr.│ │Ctx. │ │Jrny.│ │Focus│ │Topo.│         │
│  └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘         │
│     │       │       │       │       │       │              │
│  ───┼───────┼───────┼───────┼───────┼───────┼──────────    │
│     │       │       │       │       │       │              │
│  ┌──┴───────┴───────┴───────┴───────┴───────┴──┐           │
│  │         INITIATIVE A: "Reduce Churn"         │           │
│  │  L1→D1→D2→D3→S1→L2→S2→L3→S3→S4→L4→L6      │           │
│  └──┬───────┬───────┬───────┬───────┬───────┬──┘           │
│     │       │       │       │       │       │              │
│  ───┼───────┼───────┼───────┼───────┼───────┼──────────    │
│     │       │       │       │       │       │              │
│  ┌──┴───────┴───────┴───────┴───────┴───────┴──┐           │
│  │      INITIATIVE B: "Billing Redesign"        │           │
│  │  L1→D1→D2→D3→S1→L2→S2→L3→S3→S4→L4→L6      │           │
│  └──┬───────┬───────┬───────┬───────┬───────┬──┘           │
│     │       │       │       │       │       │              │
│  ───┼───────┼───────┼───────┼───────┼───────┼──────────    │
│     │       │       │       │       │       │              │
│  ┌──┴───────┴───────┴───────┴───────┴───────┴──┐           │
│  │      INITIATIVE C: "API Performance"         │           │
│  │  L1→D1→D2→D3→S1→L2→S2→L3→S3→S4→L4→L6      │           │
│  └─────────────────────────────────────────────┘           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

Each initiative has its own instance graph running through the 12 instance nodes. But they all connect UP to the 6 singleton/hybrid nodes. Those connections are where everything gets interesting.

---

## Cross-Initiative Edge Types (4)

The within-initiative edges (FEEDS, CHECKS, CONSTRAINS, TENSIONS, ENABLES, MIRRORS) still apply within each initiative's instance graph. These four new edge types describe relationships **between** initiatives:

### COMPETES (Ai → Singleton ← Bj) — Resource Contention

Two initiatives want the same scarce resource at a singleton node.

```
Initiative A (S4 Make) ──COMPETES──▶ N1 Topology ◀── Initiative B (S4 Make)
  "Both need the Onboarding Team's bandwidth."
  "One wins, one waits, or both get half."

Initiative A (L2 Decomp) ──COMPETES──▶ A1 Focus ◀── Initiative B (S3 Spec)
  "Both need human attention this sprint."
  "Attention allocated to A is attention not available for B."
```

**COMPETES is like TENSIONS but between initiatives rather than within one.** TENSIONS is a philosophical disagreement between two nodes. COMPETES is a resource scarcity problem between two initiatives.

### CONFLICTS (Ai → M2 ← Bj) — Territory Collision

Two initiatives modify the same part of shared reality (usually the codebase) in potentially incompatible ways.

```
Initiative A (S4 Make): Modifying the onboarding email flow
Initiative B (S4 Make): Modifying the onboarding email template

Both are changing the same part of M2 Territory.
Their changes may be compatible or incompatible.
```

**CONFLICTS has three severity levels:**

| Severity | Meaning | Resolution | Who Decides |
|----------|---------|------------|-------------|
| **MERGE** | Code-level collision (same files) | Technical coordination, often auto-resolvable | AI can often resolve |
| **SEMANTIC** | Behavior-level collision (same code, incompatible behaviors) | Architectural decision needed | Human architect |
| **STRATEGIC** | Direction-level collision (initiatives pull product in opposite directions) | Portfolio-level decision | Portfolio manager |

CONFLICTS is different from COMPETES:
- **COMPETES** is about **time and attention** (who gets the team this sprint?) → solved by scheduling
- **CONFLICTS** is about **territory** (can both changes coexist?) → solved by technical/strategic analysis

### AMPLIFIES (Ai ↔ Bj) — Mutual Reinforcement

Two initiatives make each other more valuable. The positive counterpart to COMPETES and CONFLICTS.

```
Initiative A: Building personalized onboarding emails
Initiative B: Building a notification preferences system

A's work makes B's work more valuable (personalized notifications).
B's work makes A's work more effective (users control what they get).
```

**AMPLIFIES initiatives should be sequenced to maximize combined value** rather than treated independently. This is a portfolio optimization opportunity.

### DEPENDS (Ai → Bj) — Cross-Initiative Precondition

One initiative's output is another initiative's precondition.

```
Initiative A: Building the user preferences API
Initiative B: Building personalized notifications (NEEDS the preferences API)

B cannot proceed past S3 Spec without A's S4 Make being ACTIVE.
```

This is a **hard cross-initiative dependency**. B's instance node has a precondition on A's instance node. Like BLOCKS within a single initiative, but across initiative boundaries.

---

## Singleton Node States

Singleton nodes have more complex state than instance nodes because they're being pulled on by multiple initiatives simultaneously.

### Partitioned State

A singleton doesn't just have a single state — it has state **per initiative** plus a **system-level state**:

```
A1 Focus (singleton):
  System state: ACTIVE
  Capacity: 100 units of human attention

  Allocated:
  ├── Initiative A: 40 units (A1 is ACTIVE for A)
  ├── Initiative B: 35 units (A1 is ACTIVE for B)
  ├── Initiative C: 15 units (A1 is ACTIVE for C)
  └── Unallocated: 10 units (reserve)

  When Initiative D arrives requesting 30 units:
  ├── COMPETES edges activate against A, B, C
  ├── Someone must lose allocation
  └── A1 for some initiative → STARVED
```

### Singleton-Specific States

In addition to the standard node states from [STATE_MODEL.md](./STATE_MODEL.md), singletons can be in:

| State | Meaning | Example |
|-------|---------|---------|
| **CONTENDED** | Multiple initiatives competing for this resource, arbitration needed | N1 Topology: 3 initiatives need Onboarding Team |
| **OVERLOADED** | More demand than capacity, everything degraded | A1 Focus: 120 units requested, 100 available |
| **STARVED** | A specific initiative's allocation has been cut to fund another | Initiative C's share of A1 Focus reduced from 15 to 5 |

### Singleton Attributes (New)

```
capacity          — total available resource units
allocation_map    — how capacity is distributed across initiatives
contention_count  — number of COMPETES edges currently active
utilization       — percentage of capacity in use
```

---

## The Portfolio Layer

The model needs a layer **above** individual initiatives that manages the collection.

### Portfolio as a Meta-Graph

The portfolio is a **meta-graph** where each initiative is a node, and the cross-initiative edges (COMPETES, CONFLICTS, AMPLIFIES, DEPENDS) are the relationships between them.

```
PORTFOLIO META-GRAPH:

  Nodes: Each initiative (A, B, C, D...)

  Edges:
  ├── A ──COMPETES──▶ B    (both need Onboarding Team)
  ├── A ──CONFLICTS──▶ B   (both modify onboarding-email-flow)
  ├── A ──AMPLIFIES──▶ B   (A's output makes B more valuable)
  └── C ──DEPENDS──▶ A     (C needs A's API)

  Singleton nodes from the initiative level become
  SHARED RESOURCE nodes at the portfolio level:
  ├── A1 Focus      → "Human Attention Pool"
  ├── N1 Topology   → "Team Capacity Pool"
  ├── M2 Territory  → "Codebase State"
  ├── M1 Value      → "Value Landscape"
  ├── M3 Context    → "Domain Structure"
  └── M4 Journey    → "User Experience"
```

### Portfolio Initiative States

Each initiative at the portfolio level has a lifecycle state:

```
PROPOSED  → Initiative identified but not committed
QUEUED    → Committed but waiting for resources
ACTIVE    → In flight, consuming resources
BLOCKED   → Waiting on DEPENDS edge from another initiative
PAUSED    → Temporarily stopped (resources redirected elsewhere)
COMPLETED → Delivered to measurement
KILLED    → Stopped before completion (bet invalidated or strategic shift)
```

```
       ┌──────────┐
       │ PROPOSED  │
       └────┬─────┘
            │ committed
            ▼
       ┌──────────┐
       │  QUEUED   │◀──────── resources freed
       └────┬─────┘          (from PAUSED)
            │ resources available
            ▼
       ┌──────────┐
  ┌────│  ACTIVE   │────┐
  │    └────┬─────┘    │
  │         │          │
  ▼         ▼          ▼
┌────────┐ ┌────────┐ ┌────────┐
│BLOCKED │ │PAUSED  │ │COMPLETED│
│(DEPENDS│ │(resource│ └────────┘
│ unmet) │ │redirect)│
└────────┘ └────────┘
  │         │
  │         └──────────▶ KILLED
  └────────────────────▶ KILLED
```

### Portfolio Health States

The portfolio as a whole has a health state:

| State | Meaning | Signal |
|-------|---------|--------|
| **BALANCED** | Resources match demand, no dangerous contention | Green — working well |
| **STRETCHED** | Demand exceeds capacity but manageable | Yellow — monitor closely |
| **OVERLOADED** | Too many active initiatives, quality degrading everywhere | Red — must reduce active count |
| **GRIDLOCKED** | Circular dependencies or irreconcilable conflicts blocking progress | Critical — structural intervention needed |

---

## Portfolio Decisions

The portfolio layer is where the hardest decisions live:

### PRIORITIZE — When COMPETES Edges Activate

```
Question: "Who gets the scarce resource?"
Inputs:
├── M1 Value Map (which initiative is highest value?)
├── D3 Bet fitness scores from L6 Measurement (which is working?)
├── AMPLIFIES edges (which combination creates most value?)
└── Strategic alignment (which fits the current direction?)
```

### SEQUENCE — When DEPENDS Edges Exist

```
Question: "What order do initiatives proceed?"
Inputs:
├── Dependency graph between initiatives
├── N1 Topology bandwidth (can we parallelize?)
├── Critical path analysis (what's blocking the most value?)
└── AMPLIFIES edges (which sequence maximizes combined value?)
```

### RESOLVE — When CONFLICTS Edges Activate

```
Question: "Which initiative yields?"
Inputs:
├── Conflict severity (MERGE vs SEMANTIC vs STRATEGIC)
├── M3 Context Map (domain boundaries)
├── L1 Outcomes (which outcome matters more?)
└── AMPLIFIES edges (does resolving for A help or hurt B?)
```

### KILL — When Measurement Shows Failure

```
Question: "Should we stop this initiative?"
Inputs:
├── D3 Bet fitness scores (is the hypothesis wrong?)
├── Sunk cost vs. future value assessment
├── AMPLIFIES edges (killing A might devalue B)
├── DEPENDS edges (killing A might block C)
└── COMPETES edges (killing A frees resources for D)
```

### SPAWN — When PULSE Detects Opportunity

```
Question: "Should we start a new initiative?"
Inputs:
├── M2 Territory (what's changed in reality?)
├── M1 Value (where's the new opportunity?)
├── A1 Focus (do we have attention budget?)
├── N1 Topology (do we have team bandwidth?)
└── Portfolio health (can we absorb another initiative?)
```

### The KILL Decision Deserves Special Attention

KILL has the most complex cascade because it affects both the initiative graph AND the portfolio meta-graph:

```
KILLING Initiative A:
  1. A's instance nodes → RETIRED (direct effect)

  2. Check DEPENDS edges:
     ├── Initiative C DEPENDS on A's S4 Make
     ├── C's precondition is now permanently unresolvable
     └── C must be PAUSED, REDESIGNED, or KILLED

  3. Check AMPLIFIES edges:
     ├── A AMPLIFIES B
     ├── B's value proposition weakens without A
     └── B might need re-evaluation (D3 Bet → STALE)

  4. Check COMPETES edges:
     ├── A was consuming 0.4 of Onboarding Team bandwidth
     ├── Initiative D (QUEUED) can now proceed
     └── N1 Topology rebalances

  5. Check CONFLICTS edges:
     ├── A's conflict with B is resolved (dead initiative can't conflict)
     └── B's CONTESTED state may clear

  6. Update singletons:
     ├── M2 Territory → may need rollback if A modified codebase
     ├── A1 Focus → attention freed (positive cascade)
     └── M1 Value → value landscape shifts
```

**The blast radius of a KILL cascades through both the initiative graph AND the portfolio meta-graph.** This is why KILL is always HUMAN DECIDES in the authority matrix.

---

## Portfolio Arbitration Authority

This extends the authority matrices from [STATE_MODEL.md](./STATE_MODEL.md) to the portfolio level.

### The Portfolio Authority Matrix

Authority is determined by **impact scope** × **strategic weight**:

```
                    TACTICAL         STRUCTURAL       STRATEGIC
                ┌────────────────┬────────────────┬────────────────┐
 SINGLE         │ AI decides     │ AI proposes    │ Human decides  │
 INITIATIVE     │                │ human confirms │                │
                ├────────────────┼────────────────┼────────────────┤
 MULTIPLE       │ AI proposes    │ Human decides  │ Human decides  │
 INITIATIVES    │ human approves │ AI supports    │ AI supports    │
                ├────────────────┼────────────────┼────────────────┤
 SYSTEMIC       │ Human decides  │ Human decides  │ Human decides  │
                │ AI supports    │ AI supports    │ AI supports    │
                └────────────────┴────────────────┴────────────────┘
```

**The bottom-right quadrant is always human.** Systemic + strategic decisions are where organizational leadership is irreplaceable.

### Arbitration Strategies by Singleton

**A1 Focus (attention allocation):**
```
Strategy: Weighted by M1 Value Map position × D3 Bet fitness
├── Higher value + higher fitness = more attention
├── New initiatives get minimum viable allocation to reach L6 Measurement
└── Rebalanced every PULSE cycle

Who arbitrates: Human portfolio manager (always)
AI can: Propose allocation based on value/fitness modeling
AI cannot: Reallocate human attention without approval
```

**N1 Topology (team bandwidth):**
```
Strategy: Based on DEPENDS ordering + value priority
├── Dependencies satisfied first (DEPENDS edges)
├── Then highest value initiatives get bandwidth
├── Context-switching penalty applied (team can't work on too many things)
└── Rebalanced every PULSE cycle

Who arbitrates: Human portfolio manager + team leads
AI can: Model scenarios, predict throughput, flag context-switching risk
AI cannot: Reassign teams without approval
```

**M2 Territory (codebase conflicts):**
```
Strategy: Based on conflict severity
├── MERGE conflict: AI can often resolve automatically
├── SEMANTIC conflict: AI proposes, human architect decides
├── STRATEGIC conflict: Portfolio manager decides which initiative yields
└── Detected continuously (not just at PULSE)

Who arbitrates: Depends on conflict type (see severity levels above)
AI can: Detect conflicts, resolve merge conflicts, propose solutions
AI cannot: Resolve strategic conflicts
```

---

## Cross-Initiative Cascade Rules

The within-initiative cascade rules from [STATE_MODEL.md](./STATE_MODEL.md) are extended with cross-initiative cascades:

### DEPENDS Cascade
```
IF Initiative A's node goes STALE or FAILING
AND Initiative B has a DEPENDS edge on that node
THEN B's dependent node → BLOCKED
     B may need to be PAUSED at portfolio level
```

### CONFLICTS Cascade
```
IF Initiative A's S4 Make modifies M2 Territory
THEN check all CONFLICTS edges to other initiatives
     MERGE conflict → auto-resolve or flag for coordination
     SEMANTIC conflict → block both initiatives until resolved
     STRATEGIC conflict → escalate to portfolio for RESOLVE decision
```

### COMPETES Cascade
```
IF Initiative A consumes more of a singleton's capacity
THEN check all COMPETES edges to other initiatives
     Other initiatives sharing that singleton may become STARVED
     STARVED initiatives → edge quality DEGRADED → slower FLOW
```

### AMPLIFIES Cascade (Positive)
```
IF Initiative A's L6 Measurement shows success
THEN check AMPLIFIES edges to other initiatives
     Amplified initiatives' value increases
     May trigger PRIORITIZE decision to invest more in amplified initiatives
```

### KILL Cascade
```
IF Initiative is KILLED
THEN cascade through ALL cross-initiative edge types:
     DEPENDS  → dependent initiatives BLOCKED or KILLED
     AMPLIFIES → amplified initiatives devalued (D3 Bet → STALE)
     COMPETES  → freed resources available for QUEUED initiatives
     CONFLICTS → conflict resolved (dead initiative can't conflict)
```

---

## PULSE at Portfolio Level

The PULSE mechanism from [STATE_MODEL.md](./STATE_MODEL.md) now operates at two levels:

### Initiative-Level PULSE (from State Model)
```
What: Map nodes refresh → detect drift → trigger RIPPLE
Tempo: Per initiative lifecycle
Purpose: Keep individual initiative honest
```

### Portfolio-Level PULSE (New)

Four cadences for portfolio-wide health checks:

**Resource PULSE (weekly):**
```
├── A1 Focus: Is attention allocated well? Any STARVED initiatives?
├── N1 Topology: Is bandwidth allocated well? Context-switching costs?
└── Output: Rebalance recommendations
```

**Conflict PULSE (continuous):**
```
├── M2 Territory: Are any initiatives modifying the same territory?
├── M3 Context: Have domain boundaries shifted?
└── Output: Conflict alerts, merge coordination triggers
```

**Value PULSE (monthly):**
```
├── M1 Value: Has the value landscape shifted?
├── L6 Measurements across all initiatives: What's working?
├── D3 Bet fitness scores: Which bets are paying off?
├── AMPLIFIES edges: Are we maximizing combined value?
└── Output: PRIORITIZE, KILL, SPAWN decisions
```

**Strategy PULSE (quarterly):**
```
├── All maps: Has the world changed?
├── Portfolio composition: Are we working on the right mix?
├── Strategic conflicts: Are initiatives pulling in contradictory directions?
└── Output: Portfolio rebalancing, new initiative proposals
```

---

## Summary

```
┌────────────────────────────────────────────────────────────┐
│              MULTI-INITIATIVE MODEL                         │
│                                                            │
│  GRAPH STRUCTURE:                                          │
│    Initiative layer: 12 instance nodes × N initiatives     │
│    Singleton layer:  5 shared nodes + 1 hybrid             │
│    Portfolio layer:  N initiative-nodes + cross edges      │
│                                                            │
│  NODE TYPES (3):                                           │
│    INSTANCE   — per-initiative (12 node types)             │
│    SINGLETON  — shared across initiatives (5 nodes)        │
│    HYBRID     — singleton master + initiative views (1)    │
│                                                            │
│  CROSS-INITIATIVE EDGES (4):                               │
│    COMPETES   — resource contention at singletons          │
│    CONFLICTS  — territory modification collision           │
│    AMPLIFIES  — mutual reinforcement (positive!)           │
│    DEPENDS    — cross-initiative precondition              │
│                                                            │
│  CONFLICT SEVERITY (3 levels):                             │
│    MERGE      — code-level, often auto-resolvable          │
│    SEMANTIC   — behavior-level, needs architect             │
│    STRATEGIC  — direction-level, needs portfolio manager    │
│                                                            │
│  SINGLETON STATES (added to state model):                  │
│    CONTENDED  — multiple initiatives competing             │
│    OVERLOADED — more demand than capacity                  │
│    STARVED    — initiative allocation cut                  │
│                                                            │
│  PORTFOLIO INITIATIVE STATES:                              │
│    PROPOSED → QUEUED → ACTIVE → COMPLETED                  │
│                          ↕ ↕                               │
│                    BLOCKED  PAUSED → KILLED                │
│                                                            │
│  PORTFOLIO HEALTH:                                         │
│    BALANCED → STRETCHED → OVERLOADED → GRIDLOCKED          │
│                                                            │
│  PORTFOLIO DECISIONS (5):                                  │
│    PRIORITIZE — who gets the scarce resource               │
│    SEQUENCE   — what order do initiatives proceed          │
│    RESOLVE    — which initiative yields in a conflict      │
│    KILL       — should we stop this initiative             │
│    SPAWN      — should we start a new one                  │
│                                                            │
│  PORTFOLIO AUTHORITY (impact_scope × strategic_weight):    │
│    Single+Tactical: AI decides                             │
│    Systemic+Strategic: Human decides (always)              │
│                                                            │
│  PULSE CADENCES:                                           │
│    Resource:  Weekly                                       │
│    Conflict:  Continuous                                   │
│    Value:     Monthly                                      │
│    Strategy:  Quarterly                                    │
│                                                            │
│  CROSS-INITIATIVE CASCADE RULES:                           │
│    DEPENDS:   upstream failure → downstream BLOCKED        │
│    CONFLICTS: territory collision → alert/block by type    │
│    COMPETES:  resource grab → others STARVED               │
│    AMPLIFIES: success → amplified initiatives gain value   │
│    KILL:      death → cascades through ALL edge types      │
└────────────────────────────────────────────────────────────┘
```

---

## Relationship to Other Models

This is the third of three companion documents:

| Document | Metaphor | What It Defines |
|----------|----------|----------------|
| [GRAPH_MODEL.md](./GRAPH_MODEL.md) | **Anatomy** — what exists | 18 nodes, 6 edge types, attributes, operations |
| [STATE_MODEL.md](./STATE_MODEL.md) | **Physiology** — how it moves | States, transitions, authority, cascades, movement types |
| **PORTFOLIO_MODEL.md** (this document) | **Ecology** — how multiple organisms interact | Instance vs singleton nodes, cross-initiative edges, portfolio decisions, arbitration |

The graph model describes **one initiative's structure**. The state model describes **one initiative's dynamics**. The portfolio model describes **how many initiatives coexist in shared reality**.
