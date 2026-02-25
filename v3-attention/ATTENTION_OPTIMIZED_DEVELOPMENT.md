# V3: Attention-Optimized Development

> **Core Question: "How do we optimize scarce human attention for alignment?"**

## The Philosophical Difference

- V1 asks: "What's the right **structure**?"
- V2 asks: "What's the right **flow**?"
- V3 asks: "What deserves human **attention**?"

## Why This Dimension Matters

AI context windows and human working memory are **structurally the same constraint**: both are finite buffers that determine how much can be reasoned about at once. This isn't a metaphor — it's a design parallel:

| Property | Human Working Memory | AI Context Window |
|---|---|---|
| Capacity | 7±2 chunks (Miller) | ~128K tokens |
| Overload behavior | Errors, shortcuts, fatigue | Hallucination, lost coherence |
| Optimization strategy | Chunk, offload, focus | Decompose, preload, constrain |
| Refresh mechanism | Sleep, breaks, re-reading | New session, re-prompt |
| Failure mode | "I forgot why we're doing this" | "Context lost, generating off-spec" |

The implication: **designing for AI context windows and designing for human cognitive limits follow the same principles.** Solve one, you've solved both.

---

## Foundation Models

| Model | Era | Core Insight | V3 Application |
|---|---|---|---|
| Cognitive Load Theory (Sweller, 1988) | 80s | Three types of load: intrinsic, extraneous, germane | Classify every SDLC activity by load type; eliminate extraneous |
| Miller's Law (1956) | 50s | Working memory holds 7±2 chunks | Chunk all artifacts to fit human review capacity |
| Attention Economics (Simon, 1971) | 70s | "A wealth of information creates a poverty of attention" | AI creates information wealth; human attention is the scarce currency |
| Distributed Cognition (Hutchins, 1995) | 90s | Thinking happens across people, tools, and artifacts | AI is a cognitive partner, not just a tool |
| Cognitive Dimensions (Green & Petre, 1996) | 90s | Framework for evaluating notations and tools | Evaluate SDLC artifacts for cognitive accessibility |

---

## The V3 Model: Four Cognitive Modes

Instead of layers or stations, V3 organizes around **four modes of human cognitive engagement**, each requiring different artifact design and AI support:

```
┌─────────────────────────────────────────────────────┐
│            ATTENTION-OPTIMIZED DEVELOPMENT           │
│                                                      │
│  ┌────────────┐   ┌────────────┐                     │
│  │   FOCUS    │──→│   CHUNK    │                     │
│  │            │   │            │                     │
│  │ "What      │   │ "Package   │                     │
│  │  deserves  │   │  it for    │                     │
│  │  my        │   │  bounded   │                     │
│  │  attention"│   │  cognition"│                     │
│  └────────────┘   └─────┬──────┘                     │
│       ▲                 │                            │
│       │                 ▼                            │
│  ┌────────────┐   ┌────────────┐                     │
│  │  REFRESH   │◄──│  OFFLOAD   │                     │
│  │            │   │            │                     │
│  │ "Re-       │   │ "Delegate  │                     │
│  │  evaluate  │   │  everything│                     │
│  │  mental    │   │  else to   │                     │
│  │  models"   │   │  AI"       │                     │
│  └────────────┘   └────────────┘                     │
│                                                      │
│  Cycle: Focus → Chunk → Offload → Refresh → Focus   │
└─────────────────────────────────────────────────────┘
```

---

## Cognitive Load Classification

Every SDLC activity falls into one of three cognitive load types (Sweller):

### Intrinsic Load (the problem itself — can't be eliminated)
```
- Understanding the business domain
- Making value judgments ("is this the right outcome?")
- Resolving ambiguity in requirements
- Evaluating trade-offs between competing goods
- Judging whether something "feels right" for users
```

### Extraneous Load (process overhead — ELIMINATE)
```
- Writing boilerplate code                    → AI generates
- Maintaining documentation                   → AI generates
- Running test suites manually                → CI/CD automates
- Formatting, linting, style debates          → AI handles
- Tracking traceability manually              → AI maintains
- Reading code to understand what it does     → AI summarizes
- Context-switching between unrelated tasks   → WIP limits prevent
```

### Germane Load (building understanding — MAXIMIZE)
```
- Reviewing AI-generated contracts for alignment
- Discussing decomposition boundaries with the team
- Examining metrics to understand user behavior
- Updating mental models when assumptions break
- Learning from production incidents
```

**V3's prime directive: eliminate extraneous load, so all human attention goes to intrinsic + germane load.** AI handles everything else.

---

## V1/V2/V3 Comparison

| Dimension | V1 (Structure) | V2 (Flow) | V3 (Cognitive) |
|---|---|---|---|
| **Metaphor** | Architecture | Production system | Mental architecture |
| **Organizing unit** | Layer | Station | Cognitive mode |
| **Key question** | "Is each layer well-defined?" | "Is alignment flowing?" | "Is human attention well-spent?" |
| **Optimization target** | Artifact completeness | Throughput | Cognitive efficiency |
| **Failure mode** | Over-specified but misaligned | Fast but unvalidated | Fatigued, distracted, overwhelmed |
| **AI role** | Generate from specs | Execute non-constraints | Handle all extraneous load |
| **Human role** | Author/approve at each layer | Unblock the constraint | Focus only on intrinsic decisions |
| **Best for** | Greenfield, regulated | Evolving products | Teams drowning in information |

→ See individual mode documents: [Focus](FOCUS.md) · [Chunk](CHUNK.md) · [Offload](OFFLOAD.md) · [Refresh](REFRESH.md)
