# V6: Network-Aligned Development

> **Core Question: "Who needs to agree with whom — and how do we minimize the cost of that agreement?"**

## The Philosophical Difference

- V1-V5 focus on artifacts, flow, cognition, decisions, and delegation
- V6 focuses on **people**: how teams, AI, and stakeholders communicate to maintain alignment

## Why This Dimension Matters

Conway's Law is not a suggestion — it's an observation with the force of gravity: **system structure mirrors communication structure.** If you want a well-decomposed system, you need a well-decomposed organization. AI is now a participant in that communication network, and its arrival changes the topology.

---

## Foundation Models

| Model | Era | Core Insight | V6 Application |
|---|---|---|---|
| Conway's Law (1967) | 60s | System structure mirrors org structure | Design communication intentionally; the system follows |
| Team Topologies (Skelton & Pettit, 2019) | 10s | Four team types, three interaction modes | Map teams to AI interaction patterns |
| Sociotechnical Systems (Trist, 1951) | 50s | Can't optimize technical without optimizing social | AI changes the social system, not just the technical |
| Dunbar's Number (~150) | 90s | Cognitive limit on stable social relationships | Limits on how many coordination channels work |
| Brooks's Law (1975) | 70s | Adding people to a late project makes it later | Communication overhead scales quadratically |

---

## The V6 Model: Four Communication Domains

```
┌─────────────────────────────────────────────────────────┐
│              NETWORK-ALIGNED DEVELOPMENT                 │
│                                                          │
│  ┌────────────┐   ┌────────────┐                         │
│  │  TOPOLOGY  │──→│ INTERFACES │                         │
│  │            │   │            │                         │
│  │ "Who talks │   │ "How do    │                         │
│  │  to whom?" │   │  they      │                         │
│  │            │   │  talk?"    │                         │
│  └────────────┘   └─────┬──────┘                         │
│       ▲                 │                                │
│       │                 ▼                                │
│  ┌────────────┐   ┌────────────┐                         │
│  │    SYNC    │◄──│ BANDWIDTH  │                         │
│  │            │   │            │                         │
│  │ "When do   │   │ "How much  │                         │
│  │  they      │   │  can flow  │                         │
│  │  align?"   │   │  between?" │                         │
│  └────────────┘   └────────────┘                         │
└─────────────────────────────────────────────────────────┘
```

---

## AI as a Communication Node

AI is not just a tool — it's a **participant in the communication network**. It has:

```
COMMUNICATION PROPERTIES OF AI:
├── Bandwidth:    Very high (can process all artifacts simultaneously)
├── Availability: 24/7 (no meetings, no time zones, no PTO)
├── Memory:       Per-session only (context window = working memory)
├── Initiative:   None (responds, doesn't initiate)
├── Judgment:     Pattern-based (not values-based)
└── Trust:        Must be earned per activity type (see V5)
```

### How AI Changes the Communication Graph

```
BEFORE AI (N humans):
  Communication channels = N(N-1)/2
  5 people = 10 channels (manageable)
  20 people = 190 channels (Brooks's Law kicks in)

AFTER AI (N humans + AI hub):
  AI becomes a hub node — reducing point-to-point communication:
  
  BEFORE:  Dev A ←→ Dev B ←→ Dev C ←→ PM ←→ Designer
           (every pair must align directly)

  AFTER:   Dev A ←→ AI ←→ Dev B
           Dev C ←→ AI ←→ PM
           (AI mediates through shared artifacts)

  The shared artifacts (specs, contracts, glossary) ARE the communication.
  AI maintains them. Humans align through them, not through meetings.
```

### Conway's Law in the AI Era

```
TRADITIONAL CONWAY'S LAW:
  Org structure → System structure

AI-ERA CONWAY'S LAW:
  (Org structure + AI communication topology) → System structure

  If AI mediates between teams through shared specs:
    → System boundaries align to spec boundaries
    → Which align to context window boundaries
    → Which align to team cognitive boundaries

  It's Conway's Law all the way down.
```

---

## Team Topologies Mapping

Skelton & Pettit define four team types. Each has a different AI interaction pattern:

### Stream-Aligned Teams (deliver value directly)
```
AI interaction: AI is a team member generating code, tests, docs
Communication: Team ←→ AI ←→ Artifacts
Context window: Contains team's outcome + team's specs
```

### Enabling Teams (help others adopt new capabilities)
```
AI interaction: AI helps create learning materials, documentation, examples
Communication: Enabling team ←→ AI ←→ Stream-aligned team artifacts
Context window: Contains target team's context + enabling knowledge
```

### Platform Teams (provide self-service capabilities)
```
AI interaction: AI maintains platform contracts, generates SDKs, monitors health
Communication: Platform team ←→ AI ←→ Platform specs ←→ Consumer teams
Context window: Contains platform contracts + consumer usage patterns
```

### Complicated-Subsystem Teams (deep specialist knowledge)
```
AI interaction: AI assists but human expertise dominates (likely T2)
Communication: Specialist team ←→ AI ←→ Interface contracts
Context window: Contains deep domain knowledge that may exceed normal budgets
```

---

## Context Window as Communication Channel

The context window is literally a **communication channel** between humans and AI:

```
HUMAN → (loads context) → AI CONTEXT WINDOW → (generates output) → HUMAN

Channel capacity:   ~128K tokens
Signal:             Specs, contracts, outcome definition
Noise:              Irrelevant code, stale docs, ambiguous language
Signal-to-noise:    Determined by what you load into context

OPTIMIZE BY:
  - Loading only relevant artifacts (not "everything")
  - Using consistent terminology (glossary reduces ambiguity)
  - Structuring artifacts consistently (templates reduce parsing overhead)
  - Removing stale content (REFRESH from V3)
```

---

## Comparison

| Dimension | V1 | V2 | V3 | V4 | V5 | V6 |
|---|---|---|---|---|---|---|
| **Metaphor** | Building | Factory | Brain | Bets | Delegation | Network |
| **Unit** | Layer | Station | Mode | Decision | Trust level | Team/node |
| **Question** | "Well-defined?" | "Flowing?" | "Well-spent?" | "Well-bet?" | "Well-delegated?" | "Well-connected?" |
| **Failure** | Over-specified | Unvalidated | Overwhelmed | Wrong bet | Over/under-trust | Miscommunication |

→ See domain documents: [Topology](TOPOLOGY.md) · [Interfaces](INTERFACES.md) · [Bandwidth](BANDWIDTH.md) · [Sync](SYNC.md)
