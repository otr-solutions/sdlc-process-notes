# Domain 1: TOPOLOGY

> **"Design who talks to whom — the system will follow."**

## Purpose

Intentionally design the communication structure between humans, teams, and AI. Conway's Law guarantees the system architecture will mirror this topology. Make it mirror the architecture you want.

---

## The Topology Decision

### Option A: Hub-and-Spoke (AI as mediator)
```
        Team A
          │
          ▼
  Team C ←→ AI HUB ←→ Team B
          ▲
          │
        Team D

Best when:
  - Teams are distributed (time zones, locations)
  - Shared artifacts are the primary coordination mechanism
  - Teams need to work asynchronously
  - System has clear, well-defined interfaces between components
```

### Option B: Mesh with AI Augmentation
```
  Team A ←→ Team B
    ↕    ╲  ╱   ↕
   AI    ╳    AI
    ↕  ╱  ╲    ↕
  Team C ←→ Team D

Best when:
  - Teams are co-located or closely collaborating
  - Emergent behavior at seams requires real-time human discussion
  - The domain is complex and poorly understood
  - Innovation happens at team intersections
```

### Option C: Layered (AI as platform)
```
  Stream Teams: A, B, C (each with embedded AI)
        │           │         │
        └─────┬─────┘
              ▼
  Platform Team + AI Platform (shared services)
              ▼
  Complicated-Subsystem Team + Specialist AI

Best when:
  - Clear platform/consumer relationship exists
  - Self-service is the primary interaction mode
  - Scale requires standardization
```

---

## Topology and Context Windows

Each topology implies a different context loading strategy:

```
HUB-AND-SPOKE:
  Every team's AI loads: shared glossary + their own specs + interface contracts
  Teams never load each other's implementation details
  → Clean information hiding at team boundaries

MESH:
  AI loads broader context: adjacent team specs + shared concerns
  More context overhead but better for catching integration issues
  → Trades context budget for alignment awareness

LAYERED:
  Platform AI loads: all consumer contracts + platform internals
  Consumer AI loads: platform contracts + own specs
  → Asymmetric context loading reflects asymmetric relationships
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Choose topology | Decides based on org structure and goals | Proposes based on dependency analysis |
| Maintain topology | Adjusts as teams/projects evolve | Flags when communication patterns don't match design |
| Enforce boundaries | Sets interaction norms | Limits cross-boundary context loading |
| Detect Conway drift | Reviews whether system matches org | Analyzes coupling vs. team ownership |
