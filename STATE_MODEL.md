# State Model: How the Graph Moves

> **The graph model tells you what exists. The state model tells you how it moves.**

This document defines the states, transitions, authority model, and movement patterns for the graph defined in [GRAPH_MODEL.md](./GRAPH_MODEL.md). Together, they form the complete operating system for the AI Software Factory.

---

## Node States (7)

Every node in the graph can be in one of the following states:

### Universal States

| State | Meaning | Example |
|-------|---------|---------|
| **EMPTY** | Node hasn't been activated yet. No artifact exists. | L1 Outcome: "We haven't defined what we're trying to achieve" |
| **DRAFT** | An artifact exists but hasn't been validated. | L1 Outcome: "We have a candidate outcome statement, not yet validated" |
| **ACTIVE** | Artifact is validated and being used by other nodes. | L1 Outcome: "Outcome is agreed and driving downstream work" |
| **RETIRED** | Artifact has been superseded or initiative completed. Kept for history. | D3 Bet: "Bet resolved — we learned it was right or wrong" |

### Conditional States

These only apply when specific conditions are met:

| State | Meaning | Applies When | Example |
|-------|---------|-------------|---------|
| **STALE** | Reality has drifted since the artifact was last validated. | Map nodes (time decay), Definition nodes (learning invalidates), any node when MIRRORS edge detects divergence | M2 Territory: "Codebase has changed since last scan" |
| **CONTESTED** | A TENSIONS edge is active and unresolved. Neither side has yielded. | Any node connected by a TENSIONS edge where the opposing node disagrees | L2 Decomposition: "Focus budget says 3 units, decomposition created 8" |
| **FAILING** | A CHECKS edge has returned a failure signal. | Execution and measurement nodes, when output doesn't match reference | S4 Make: "What we built isn't achieving the defined outcome" |

---

## Edge States (4)

Edges aren't static connections — they fire, succeed, fail, and degrade:

| State | Meaning | Example |
|-------|---------|---------|
| **INACTIVE** | Edge exists structurally but isn't currently firing. | S3 Spec ──FEEDS──▶ S4 Make: "Spec isn't done yet, nothing to feed" |
| **ACTIVE** | Information or constraint is flowing along this edge. | S3 Spec ──FEEDS──▶ S4 Make: "Spec is feeding into the build process" |
| **DEGRADED** | Edge is firing but signal quality is poor. | "Spec was poorly encoded, builders are misinterpreting" |
| **BROKEN** | Edge should be firing but isn't. | "Spec exists but build team never received it" |

### Edge State × Edge Type

Not all edge types experience all states the same way:

| Edge Type | INACTIVE | ACTIVE | DEGRADED | BROKEN |
|-----------|----------|--------|----------|--------|
| FEEDS | No input available | Information flowing | Signal quality poor (v7 attributes low) | Source node STALE or EMPTY |
| CHECKS | Neither node active | Comparison running | Reference poorly articulated | Reference node unavailable |
| CONSTRAINS | Constraint not yet relevant | Boundaries being enforced | Boundaries unclear or ambiguous | Constraining node STALE |
| TENSIONS | Neither node active | Both active, negotiation needed | Tension ignored (one side winning by default) | Tension exists but nobody managing it |
| ENABLES | Capability not yet needed | Channel/conditions established | Channel partially functional | Channel missing or broken |
| MIRRORS | Neither node active | Both active, resonance maintained | Divergence detected but not addressed | One side updated, other unaware |

**Critical insight**: A TENSIONS edge in DEGRADED state means the tension is being resolved by accident (one side winning by default). This is almost always bad — the hub should flag DEGRADED tensions as urgently as BROKEN edges.

---

## Node State Transitions

### The State Machine

```
                    ┌──────────────┐
                    │    EMPTY     │
                    └──────┬───────┘
                           │ FEEDS input arrives
                           ▼
                    ┌──────────────┐
          ┌────────│    DRAFT     │────────┐
          │        └──────┬───────┘        │
          │               │ CHECKS pass    │ CHECKS fail
          │               ▼                ▼
          │        ┌──────────────┐  (back to EMPTY)
          │        │    ACTIVE    │
          │        └──┬───┬───┬──┘
          │           │   │   │
          │    ┌──────┘   │   └──────┐
          │    ▼          ▼          ▼
     ┌────────────┐ ┌──────────┐ ┌──────────┐
     │   STALE    │ │CONTESTED │ │ FAILING  │
     └─────┬──────┘ └────┬─────┘ └────┬─────┘
           │              │            │
           │ refresh      │ resolved   │ corrected
           ▼              ▼            ▼
     (back to DRAFT) (back to ACTIVE) (back to ACTIVE or STALE)
                              │
                              │ initiative complete
                              ▼
                       ┌──────────────┐
                       │   RETIRED    │
                       └──────────────┘
```

### Transition Rules

| From | To | Trigger | Example |
|------|----|---------|---------|
| EMPTY | DRAFT | FEEDS edge delivers input, or human/AI initiates | D1 Sense goes DRAFT when someone starts assessing |
| DRAFT | ACTIVE | CHECKS edge validates the draft against a reference | L1 Outcome goes ACTIVE when it passes alignment review |
| DRAFT | EMPTY | Validation fails, artifact discarded | D3 Bet returns to EMPTY when frame changes fundamentally |
| ACTIVE | STALE | MIRRORS detects divergence, time-based decay, or L6 Measurement reveals drift | M2 Territory goes STALE when D1 Sense detects reality has changed |
| ACTIVE | CONTESTED | TENSIONS edge activates and opposing node disagrees | L2 Decomposition goes CONTESTED when A1 Focus pushes back |
| ACTIVE | FAILING | CHECKS edge returns fail | S4 Make goes FAILING when L6 Measurement shows outcomes aren't met |
| STALE | DRAFT | Refresh operation fires (A4 Refresh, D4 Learn, E4 Adapt) | M2 Territory goes DRAFT when someone updates it |
| CONTESTED | ACTIVE | Human resolves the tension | L2 Decomposition revised and returns to ACTIVE |
| FAILING | ACTIVE | P4 Correct succeeds, or issue is fixed | S4 Make returns to ACTIVE after correction |
| FAILING | STALE | Correction reveals deeper staleness upstream | S4 Make goes STALE when investigation shows spec was the problem |
| ACTIVE | RETIRED | Initiative completes or node is superseded | D3 Bet goes RETIRED after D4 Learn processes results |

---

## Edge State Transitions

| From | To | Trigger | Example |
|------|----|---------|---------|
| INACTIVE | ACTIVE | Both connected nodes reach relevant states | S3→S4 FEEDS activates when S3 reaches ACTIVE |
| ACTIVE | DEGRADED | Signal quality attributes drop below threshold | encode_quality < 0.5 on a FEEDS edge |
| ACTIVE | BROKEN | Connected node goes EMPTY or STALE, or edge is blocked | S3 goes STALE, so FEEDS to S4 breaks |
| DEGRADED | ACTIVE | P4 Correct operation fires and succeeds | Team re-syncs, spec re-transmitted |
| BROKEN | ACTIVE | Root cause resolved | Blocked node becomes ACTIVE again |

---

## Transition Authority

State transitions aren't all equal. Some should be automatic and some require human approval. Authority is determined by two factors: **blast radius** and **reversibility**.

### The Authority Matrix

```
                    REVERSIBLE           IRREVERSIBLE
                ┌───────────────────┬───────────────────┐
 LOW BLAST      │ AI ACTS           │ AI PROPOSES       │
 RADIUS         │ Human notified    │ Human confirms    │
                │                   │                   │
                │ "M2 is stale"     │ "Retire D3 Bet"   │
                ├───────────────────┼───────────────────┤
 HIGH BLAST     │ AI PROPOSES       │ HUMAN DECIDES     │
 RADIUS         │ Human approves    │ AI supports       │
                │                   │                   │
                │ "Cascade stale    │ "Kill initiative,  │
                │  back to S1"      │  results are bad"  │
                └───────────────────┴───────────────────┘
```

### Computing Blast Radius

Blast radius isn't a fixed property — it's **computed from the graph** by counting how many nodes a state change would affect through cascade rules:

```
BLAST RADIUS = count of nodes affected by cascade rules

Example — S3 Spec → STALE:
  ├── Forward FEEDS: S4 Make → affected, L4 Generation → affected
  ├── No backward cascade (spec going stale doesn't invalidate shape)
  └── Blast radius: 2 nodes → LOW

Example — D3 Bet → STALE:
  ├── Forward FEEDS: S1 Intent → affected
  │   └── S1 → S2 Shape → affected
  │       └── S2 → L3 Contracts → affected
  │           └── L3 → S3 Spec → affected
  │               └── S3 → S4 Make, L4 Gen → affected
  ├── TENSIONS: M2 Territory → check needed
  └── Blast radius: 6-7 nodes → HIGH (human must approve)
```

**The graph structure itself computes the authority model.** No manual permission assignment needed — traverse the graph to compute blast radius, then apply the matrix.

### Authority × Trust Level

Trust level (T1-T5) further modulates authority for specific transitions:

| Transition | T1-T2 | T3 | T4-T5 |
|-----------|-------|-----|-------|
| EMPTY → DRAFT | Human must initiate | AI can initiate, human notified | AI initiates autonomously |
| DRAFT → ACTIVE | Human validates alone | AI validates, human reviews exceptions | AI validates autonomously |
| ACTIVE → STALE (detection) | AI detects always | AI detects always | AI detects always |
| ACTIVE → STALE (response) | Human decides response | AI proposes, human approves | AI responds, human monitors |
| ACTIVE → CONTESTED | AI detects, **human resolves always** | Same | Same |
| ACTIVE → RETIRED | Human decides | AI proposes, human confirms | AI proposes, human confirms |

**Note**: CONTESTED resolution is ALWAYS human, regardless of trust level. Tensions are by definition where human judgment is irreplaceable.

---

## Preconditions

Not every node can transition at any time. Some transitions require other nodes to be in specific states first.

### Precondition Types

| Type | Meaning | Hub Behavior |
|------|---------|-------------|
| **HARD** | Node stays BLOCKED until precondition is met. Cannot proceed. | Hub prevents the transition |
| **SOFT** | Node can proceed, but quality will suffer. | Hub warns, allows transition, marks affected edges as DEGRADED |

### Key Preconditions

| Node Transition | Requires (HARD) | Benefits From (SOFT) |
|----------------|-----------------|---------------------|
| L2 Decomposition: EMPTY → DRAFT | L1 Outcome is ACTIVE | M3 Context is ACTIVE, A1 Focus is ACTIVE |
| S3 Spec: EMPTY → DRAFT | L2 Decomposition is ACTIVE, S2 Shape is ACTIVE | L3 Contracts is ACTIVE |
| S4 Make: EMPTY → DRAFT | S3 Spec is ACTIVE, trust_level assigned | N1 Topology shows available team with bandwidth |
| L6 Measurement: EMPTY → DRAFT | S4 Make is ACTIVE, OR D3 Bet has falsifiable prediction | M4 Journey is ACTIVE (for context) |
| D3 Bet: EMPTY → DRAFT | D2 Frame is ACTIVE | M1 Value is ACTIVE |
| L3 Contracts: EMPTY → DRAFT | S2 Shape is ACTIVE | N1 Topology is ACTIVE, M3 Context is ACTIVE |

### Concurrency Rules

Multiple nodes can be DRAFT or ACTIVE simultaneously. The preconditions determine what's safe:

```
CAN be parallel (no precondition relationship):
├── D1 Sense + M2 Territory refresh (they MIRROR, not BLOCK)
├── M1 Value Map + M4 Journey Map (independent map types)
├── Multiple map nodes updating simultaneously

MUST be sequential (HARD preconditions):
├── L1 Outcome → L2 Decomposition → S3 Spec → S4 Make
├── D1 Sense → D2 Frame → D3 Bet → S1 Intent
├── S1 Intent → S2 Shape

CAN overlap (SOFT preconditions — parallel but risky):
├── S2 Shape + L3 Contracts (shaping while defining contracts)
├── S3 Spec + S4 Make (specifying while starting to build)
└── The state model ALLOWS this but marks edges as DEGRADED
```

**This maps to real-world behavior.** Teams always work on things in parallel, sometimes "incorrectly." The state model doesn't prevent that — it makes the **risk visible**.

---

## Movement Types (3)

The graph has three fundamentally different kinds of state change:

### FLOW — Forward Progression

```
Direction: Forward through FEEDS edges
Driven by: Nodes completing (DRAFT → ACTIVE), artifacts produced
Tempo:     Deliberate, work-driven
Purpose:   Work gets done
```

FLOW is the "happy path." Nodes activate in sequence, edges fire, work moves from definition through shaping to execution to measurement.

### RIPPLE — Change Propagation

```
Direction: Outward from point of change, along connected edges
Driven by: CHECKS failures, TENSIONS activation, STALE detection
Tempo:     Immediate — ripples don't wait
Purpose:   System stays honest
```

RIPPLE is what happens when something changes, breaks, or is learned. State changes propagate outward from the point of change.

### PULSE — Periodic Refresh

```
Direction: From Map nodes (M1-M4) outward
Driven by: Time-based schedule, external change detection
Tempo:     Regular, time-driven
Purpose:   Reality check — "is the territory still what we think it is?"
```

PULSE is the system's heartbeat. Map nodes are periodically refreshed, and each refresh can trigger RIPPLES.

**PULSE is what makes this a factory instead of a pipeline.** Without PULSE, the system runs once and stops. With PULSE, it continuously adapts.

### The Danger State

The most dangerous state for the system is when **FLOW continues but RIPPLES are suppressed** — meaning teams keep building even though upstream nodes have gone stale.

```
DANGER: FLOW without RIPPLE
  = Building on a cracked foundation
  = The software equivalent of "everything is fine" while the building is on fire
  
The hub MUST surface this condition prominently.
```

---

## Cascade Rules

When a node changes state, the change may propagate to other nodes through connected edges. The direction and type of cascade depends on the edge type:

| Cascade Type | Direction | Edge Type | Rule |
|-------------|-----------|-----------|------|
| **Staleness** | BACKWARD along FEEDS | FEEDS | If upstream artifact was DEPENDENT on the now-stale node's context, cascade. If upstream stands on its own, contain. |
| **Staleness** | FORWARD along MIRRORS | MIRRORS | If one mirror goes stale, check the other — it's probably stale too. |
| **Contested** | ALONG TENSIONS | TENSIONS | If one side of a tension becomes contested, the other side is contested too (by definition). |
| **Activation** | FORWARD along FEEDS | FEEDS | When a node goes ACTIVE, downstream nodes can begin transitioning from EMPTY/BLOCKED to DRAFT. |

### Cascade Containment

Not every state change should cascade. The key question is: **does the downstream artifact still hold independently?**

```
CASCADES (downstream artifact was derived from upstream):
  D3 Bet → STALE means S1 Intent → STALE
  Because intent was DERIVED from the bet. Wrong bet = wrong intent.

CONTAINED (downstream artifact stands on its own):
  S4 Make → FAILING does NOT mean S3 Spec → STALE
  Because the spec might be fine — failure might be in execution.
  The CHECKS edge tells you WHERE the failure is.
```

---

## Lifecycle Walkthrough: Complete Initiative

To demonstrate how all states, transitions, and movements work together:

```
TIME 0: Initiative Begins (PULSE triggers)
─────────────────────────────────────────────
PULSE fires: M2 Territory shows opportunity
  M2 Territory: ACTIVE (refreshed recently)
  D1 Sense: EMPTY → DRAFT (AI detects pattern via MIRRORS)
  D1 Sense: DRAFT → ACTIVE (human validates assessment)

TIME 1: Framing (FLOW)
─────────────────────────────────────────────
  D1 Sense FEEDS D2 Frame
    D2 Frame: EMPTY → DRAFT (AI proposes frame)
    D2 Frame: DRAFT → ACTIVE (human approves: "complex, experiment")
  
  D2 Frame FEEDS D3 Bet
    D3 Bet: EMPTY → DRAFT (human writes hypothesis)
    D3 Bet: DRAFT → ACTIVE (team agrees)
    
  TENSION activates: D3 Bet ◀──TENSIONS──▶ M2 Territory
    Both nodes → CONTESTED
    Human resolves: "Bet revised to account for constraint"
    Both nodes → ACTIVE

TIME 2: Definition (FLOW)
─────────────────────────────────────────────
  D3 Bet FEEDS S1 Intent
    S1 Intent: EMPTY → DRAFT → ACTIVE
    
  M2 Territory FEEDS L1 Outcome
    L1 Outcome: EMPTY → DRAFT (AI drafts from territory + bet)
    L1 Outcome: DRAFT → ACTIVE (human validates)

TIME 3: Shaping (FLOW + TENSION)
─────────────────────────────────────────────
  L1 Outcome FEEDS L2 Decomposition
  M3 Context CONSTRAINS L2 Decomposition
    L2 Decomposition: EMPTY → DRAFT (AI proposes breakdown)
    
  TENSION activates: A1 Focus ◀──TENSIONS──▶ L2 Decomposition
    "8 units created. Focus budget says 3 max."
    Human resolves: "3 this cycle, defer 5"
    Both → ACTIVE

  S1 Intent FEEDS S2 Shape → ACTIVE
  S2 Shape FEEDS L3 Contracts → ACTIVE

TIME 4: Specification (FLOW + CHECKS)
─────────────────────────────────────────────
  L2 Decomposition FEEDS S3 Spec
  L3 Contracts FEEDS S3 Spec
    S3 Spec: EMPTY → DRAFT (AI generates at T2)
    
  CHECKS: S3 Spec against S1 Intent
    Signal: encode_quality=0.8, decode_quality=0.9
    Check PASSES
    S3 Spec: DRAFT → ACTIVE

TIME 5: Execution (FLOW)
─────────────────────────────────────────────
  S3 Spec FEEDS S4 Make / L4 Generation
    L4 Generation: EMPTY → DRAFT (AI generates at T2)
    S4 Make: EMPTY → DRAFT → ACTIVE

TIME 6: Measurement (FLOW → RIPPLE)
─────────────────────────────────────────────
  L6 Measurement: EMPTY → DRAFT (early metrics)
  
  CHECKS: L6 measures S4 Make against L1 Outcome
    Result: Partially met (fitness = 0.6)
    
  D4 Learn fires on D3 Bet (fitness = 0.6)
  L6 Measurement: DRAFT → ACTIVE

TIME 7: Evolution (RIPPLE + PULSE)
─────────────────────────────────────────────
  E1 Vary: Create branch for alternative approach
  E2 Select: Will compare after next cycle
  
  L6 Measurement FEEDS M2 Territory → refreshed
  L6 Measurement FEEDS M1 Value → updated
  L6 Measurement FEEDS A1 Focus → attention released
  A4 Refresh fires on A1 Focus
  
  M2 Territory: ACTIVE → STALE → DRAFT → ACTIVE

TIME 8: Next Cycle (PULSE triggers)
─────────────────────────────────────────────
  PULSE fires on refreshed maps
  D1 Sense picks up new signals
  Loop continues with better maps and learned fitness data
```

### Key Observations from the Walkthrough

1. **CONTESTED is the most interesting state** — every tension pause was a high-value human judgment moment
2. **Time 6-7 has the highest fan-out** — L6 Measurement triggers feeds to four nodes plus three operations. This is the most complex moment for orchestration.
3. **Trust level controls clock speed** — at T2, every DRAFT → ACTIVE requires human review. At T4, most transitions are automatic. Higher trust = faster FLOW.
4. **PULSE is the hidden driver** — the cycle starts and restarts because maps are refreshed. Without PULSE, the system runs once and stops.

---

## Summary

```
┌────────────────────────────────────────────────────────┐
│                    STATE MODEL                          │
│                                                        │
│  NODE STATES (7):                                      │
│    EMPTY → DRAFT → ACTIVE → RETIRED                    │
│                      ↕ ↕ ↕                             │
│               STALE  CONTESTED  FAILING                │
│                                                        │
│  EDGE STATES (4):                                      │
│    INACTIVE → ACTIVE ⇄ DEGRADED                        │
│                 ↕                                       │
│              BROKEN                                    │
│                                                        │
│  MOVEMENT TYPES (3):                                   │
│    FLOW    — forward progression (work gets done)      │
│    RIPPLE  — change propagation (system stays honest)  │
│    PULSE   — periodic refresh (reality check)          │
│                                                        │
│  TRANSITION AUTHORITY (2×2 matrix):                    │
│    blast_radius × reversibility                        │
│    Computed from graph traversal, not manual assignment │
│                                                        │
│  PRECONDITIONS (2 types):                              │
│    HARD — hub prevents transition                      │
│    SOFT — hub warns, allows, marks edges DEGRADED      │
│                                                        │
│  CASCADE RULES:                                        │
│    Staleness → BACKWARD along FEEDS                    │
│    Contested → ALONG TENSIONS                          │
│    Activation → FORWARD along FEEDS                    │
│    Staleness → FORWARD along MIRRORS                   │
│                                                        │
│  DANGER STATE:                                         │
│    FLOW continues while RIPPLES are suppressed         │
│    (building on a cracked foundation)                  │
│                                                        │
│  CLOCK SPEED:                                          │
│    Determined by trust_level                           │
│    Higher trust = faster FLOW = fewer human gates      │
└────────────────────────────────────────────────────────┘
```

---

## Relationship to Other Models

This is the second of three companion documents:

| Document | Metaphor | What It Defines |
|----------|----------|----------------|
| [GRAPH_MODEL.md](./GRAPH_MODEL.md) | **Anatomy** — what exists | 18 nodes, 6 edge types, attributes, operations |
| **STATE_MODEL.md** (this document) | **Physiology** — how it moves | States, transitions, authority, cascades, movement types |
| [PORTFOLIO_MODEL.md](./PORTFOLIO_MODEL.md) | **Ecology** — how multiple organisms interact | Instance vs singleton nodes, cross-initiative edges, portfolio decisions, arbitration |

The graph is the **anatomy**. The state model is the **physiology**. The portfolio model is the **ecology**.

### Extensions in Portfolio Model

The portfolio model extends several concepts from this document:
- **Node states** are extended with singleton-specific states: CONTENDED, OVERLOADED, STARVED
- **Portfolio initiative states** (PROPOSED, QUEUED, ACTIVE, BLOCKED, PAUSED, COMPLETED, KILLED) operate at a higher level than node states
- **Cascade rules** are extended with cross-initiative cascades along DEPENDS, CONFLICTS, COMPETES, AMPLIFIES, and KILL edges
- **PULSE** is extended to four portfolio-level cadences: Resource (weekly), Conflict (continuous), Value (monthly), Strategy (quarterly)
- **Authority matrix** is extended from blast_radius × reversibility to impact_scope × strategic_weight
