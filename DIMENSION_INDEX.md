# SDLC in the AI Era: A Multi-Dimensional Framework

## The Problem (Constant Across All Dimensions)

Three objectives, one constraint:

1. **Manage alignment** between intent and implementation
2. **Deliver value**, not just ship features
3. **Work within AI context window constraints**

No single lens fully captures this problem. Each dimension below illuminates different facets — they are **complementary views of the same system**, not competing alternatives.

---

## Dimension Index

| Version | Dimension | Core Question | Metaphor | Ancestry |
|---|---|---|---|---|
| [V1](layers/) | **Structure** | "What artifacts exist and how do they relate?" | Architecture (building) | Structured Analysis, Design by Contract, Cleanroom |
| [V2](v2/) | **Flow** | "How does work move through the system?" | Production line (factory) | Theory of Constraints, V-Model, Kanban, PDCA |
| [V3](v3/) | **Cognitive** | "How do we optimize scarce human attention?" | Mental architecture | Cognitive Load Theory, Miller's Law, Distributed Cognition |
| [V4](v4/) | **Risk** | "What do we do when we don't know what to build?" | Decision landscape | Cynefin, OODA Loop, Real Options, Spiral Model |
| [V5](v5/) | **Trust** | "How much autonomy does AI get, and when?" | Delegation spectrum | Principal-Agent Theory, Trust frameworks, Autonomy levels |
| [V6](v6/) | **Communication** | "Who needs to agree with whom?" | Network topology | Conway's Law, Team Topologies, Sociotechnical Systems |
| [V7](v7/) | **Information** | "Where does alignment signal degrade?" | Signal transmission | Information Theory, Shannon, SECI Model |
| [V8](v8/) | **Mapping** | "Do all participants share the same map?" | Cartography | Wardley Maps, Value Streams, DDD Context Maps |
| [V9](v9/) | **Evolution** | "How does the system adapt when the environment changes?" | Natural selection | Evolutionary Architecture, Fitness Functions, Lean Startup |

---

## How the Dimensions Relate

```
                         STRUCTURE (V1)
                        What exists?
                             │
              ┌──────────────┼──────────────┐
              │              │              │
         FLOW (V2)     MAPPING (V8)   EVOLUTION (V9)
         How it moves   Where it is    How it changes
              │              │              │
              └──────┬───────┘              │
                     │                      │
              COGNITIVE (V3) ◄──────────────┘
              Human attention
              (the scarce resource)
                     │
         ┌───────────┼───────────┐
         │           │           │
    RISK (V4)   TRUST (V5)  COMMS (V6)
    What to bet  How much     Who agrees
    on           to delegate  with whom
         │           │           │
         └───────────┼───────────┘
                     │
             INFORMATION (V7)
             Signal integrity
             (the underlying physics)
```

### Reading the Map

- **V1 (Structure)** and **V2 (Flow)** are the foundation — what exists and how it moves
- **V3 (Cognitive)** is the central node — human attention is THE constraint
- **V4, V5, V6** are strategies for managing that constraint (decisions, delegation, coordination)
- **V7 (Information)** is the underlying physics — signal preservation across all activities
- **V8 (Mapping)** and **V9 (Evolution)** are about situational awareness and adaptation

---

## Compatibility Matrix

```
              V1     V2     V3     V4     V5     V6     V7     V8     V9
              Struct Flow   Cogn   Risk   Trust  Comms  Info   Map    Evolve
V1 Struct      —     ✅     ✅     ⚡     ✅     ✅     ⚡     ✅     ⚡
V2 Flow        ✅     —     ⚡     ✅     ⚡     ✅     ⚡     ✅     ✅
V3 Cogn        ✅    ⚡      —     ✅     ✅     ✅     ✅     ⚡     ⚡
V4 Risk        ⚡    ✅     ✅      —     ✅     ✅     ✅     ✅     ✅
V5 Trust       ✅    ⚡     ✅     ✅      —     ✅     ⚡     ⚡     ✅
V6 Comms       ✅    ✅     ✅     ✅     ✅      —     ✅     ⚡     ⚡
V7 Info        ⚡    ⚡     ✅     ✅     ⚡     ✅      —     ✅     ✅
V8 Map         ✅    ✅     ⚡     ✅     ⚡     ⚡     ✅      —     ✅
V9 Evolve      ⚡    ✅     ⚡     ✅     ✅     ⚡     ✅     ✅      —

✅ = highly complementary     ⚡ = some overlap
```

---

## Using Multiple Dimensions Together

No real project should use only one lens. Recommended combinations:

**For a new team starting AI-assisted development:**
→ V1 (Structure) + V3 (Cognitive) + V5 (Trust)
"What artifacts do we need, how do we protect human attention, and how much do we delegate?"

**For an experienced team optimizing throughput:**
→ V2 (Flow) + V4 (Risk) + V7 (Information)
"How does work flow, what do we bet on, and where does signal degrade?"

**For organizational transformation:**
→ V6 (Communication) + V5 (Trust) + V8 (Mapping)
"Who needs to agree, how do we delegate, and does everyone see the same picture?"

**For long-lived products:**
→ V9 (Evolution) + V4 (Risk) + V1 (Structure)
"How do we adapt, what's the riskiest assumption, and is the structure still sound?"
