# Reviving Discarded SDLC Models for the AI Era

## The Core Insight

Many models were discarded not because they were **wrong**, but because they were **too expensive to execute with humans alone**. The cost structure has inverted:

| Era | Expensive | Cheap |
|---|---|---|
| 80s-90s | Documentation, code, testing | Human thinking time |
| 2020s+ | **Human alignment, context, attention** | Documentation, code, testing |

This means: **models designed for rigorous decomposition, specification, and traceability are suddenly viable again** — but for entirely different reasons than their creators intended.

---

## The Context Window as Architectural Constraint

This is the key nobody's talking about. AI context windows impose a **hard boundary on the unit of work**. This isn't a bug — it's a *design constraint* that forces good engineering:

- Every artifact must be **bounded and self-contained**
- Every component must have **clear interfaces**
- Every task must be **decomposable** into pieces that fit in ~128K tokens with room to reason
- Ambiguity that a human could "hold in their head across months" **must be made explicit**

This constraint is structurally identical to what the formal methods of the 80s demanded — **but for different reasons**. They wanted mathematical rigor. We need **machine-parseable clarity**.

---

## Revived Models, Reframed

### 1. Structured Analysis (DeMarco/Yourdon) → **Context-Bounded Decomposition**

**Original**: Decompose systems into data flow diagrams, process specifications, and data dictionaries. Discarded because maintaining DFDs was expensive and they drifted from code.

**Reframed**: Decompose the system into **context-window-sized units** with explicit data flows between them. Each unit has:
- Inputs (what enters the context)
- Outputs (what the unit produces)
- Dependencies (what else must be loaded into context)
- Invariants (what must remain true)

AI maintains the diagrams. Humans maintain the decomposition decisions. The "data dictionary" becomes the **shared vocabulary document** that gets loaded into every AI context.

```
System
├── Bounded Unit A (fits in one context window)
│   ├── contract.md (inputs, outputs, invariants)
│   ├── implementation/
│   └── tests/
├── Bounded Unit B
│   ├── contract.md
│   ├── implementation/
│   └── tests/
└── shared/
    ├── glossary.md (ubiquitous language)
    ├── decisions/ (ADRs)
    └── outcome-map.md (why we're building this)
```

### 2. Design by Contract (Bertrand Meyer, 1986) → **AI-Executable Specifications**

**Original**: Every method has preconditions, postconditions, and class invariants. Discarded because writing and maintaining contracts was more work than the code itself.

**Reframed**: Contracts are now the **primary artifact humans author**. AI generates the implementation *from* the contracts. The contract:
- Fits in a context window
- Is verifiable
- Communicates **intent** (alignment) not **mechanism** (implementation)
- Serves as the acceptance criteria

```
## Contract: Calculate Pricing

### Preconditions
- User has an active subscription
- Product exists in catalog
- Quantity > 0

### Postconditions
- Price reflects current tier discounts
- Tax calculated for user's jurisdiction
- Result includes line-item breakdown

### Invariants
- Total always equals sum of line items
- No negative prices
- Currency consistent throughout
```

The AI can generate code, tests, and docs from this. Humans review whether the **contract itself** is aligned to the outcome.

### 3. Spiral Model (Boehm, 1986) → **Risk-Driven Prototyping at Zero Cost**

**Original**: Each iteration evaluates risk, builds a prototype, gets feedback. Discarded because prototyping was expensive.

**Reframed**: Prototyping is nearly free. The spiral becomes:
1. **Identify the riskiest assumption** about alignment
2. **AI generates a prototype** in minutes
3. **Stakeholders validate alignment** against outcomes
4. **Discard or evolve** — there's no sunk cost
5. Repeat

The spiral model was right all along. It was just ahead of the cost curve.

### 4. Fagan Inspections → **AI-First Review, Human Alignment Review**

**Original**: Formal, structured code review with roles (moderator, reader, tester). Discarded because human time was too expensive for the formality.

**Reframed**: Two-tier review:
- **Tier 1 (AI)**: Correctness, style, test coverage, security — essentially free
- **Tier 2 (Human)**: Does this align to the outcome? Does the contract match the intent? Is the decomposition right?

Humans review **only what AI cannot**: intent, alignment, and strategic coherence.

### 5. Requirements Traceability Matrix → **Outcome Traceability**

**Original**: Map every requirement → design element → code → test. Discarded because maintaining the matrix was a full-time job.

**Reframed**: AI maintains the traceability. Humans define the outcome map:

```
Outcome: Reduce checkout abandonment by 15%
  → Opportunity: Users confused by shipping cost
    → Requirement: Show shipping estimate before checkout
      → Contract: estimate-shipping.contract.md
        → Implementation: src/shipping/estimate.ts
          → Tests: tests/shipping/estimate.test.ts
```

Every piece of code traces back to **why it exists**. AI keeps this current. Humans audit the top of the chain (is this still the right outcome?).

### 6. Literate Programming (Knuth, 1984) → **Narrative-Driven Development**

**Original**: Programs written as documents for humans to read, with code embedded. Discarded because it was impractical to maintain.

**Reframed**: AI generates the narrative layer automatically. But more importantly, the **input** to AI should be narrative:

> "This module handles user authentication. We chose JWT over sessions because our architecture is stateless across three regions. The token lifetime is 15 minutes because our security audit required it (see ADR-007)."

This narrative **is the context** that gets loaded into the AI window. It's the alignment document. Knuth was right — programs should be readable as literature. We just have a different author now.

### 7. Cleanroom Software Engineering (Mills, 1987) → **Statistical Correctness with AI-Generated Tests**

**Original**: Write specs formally, generate tests statistically, never run code until it's proven. Discarded as impractical.

**Reframed**: AI can generate thousands of test cases from contracts. Statistical coverage becomes trivial. The cleanroom insight — **don't debug, prevent defects through specification** — aligns perfectly with contract-first development.

---

## The Synthesized Model

```
┌──────────────────────────────────────────────────────┐
│        CONTEXT-ALIGNED DEVELOPMENT LIFECYCLE         │
│        (Reviving what works, for the AI era)         │
├──────────────────────────────────────────────────────┤
│                                                      │
│  LAYER 1: OUTCOME DEFINITION  [AI-proposed,          │
│  ├── What changes in the world? Human-approved]      │
│  ├── How do we measure it?                           │
│  └── Opportunity solution tree                       │
│  → See: layers/L1_OUTCOME_DEFINITION.md              │
│                                                      │
│  LAYER 2: DECOMPOSITION       [AI-proposed,          │
│  ├── Break into context-window   Human-constrained]  │
│  │   sized units                                     │
│  ├── Define boundaries & interfaces                  │
│  └── Structured Analysis, reborn                     │
│  → See: layers/L2_DECOMPOSITION.md                   │
│                                                      │
│  LAYER 3: CONTRACTS            [AI-authored,         │
│  ├── Pre/post conditions        Human-corrected]     │
│  ├── Invariants                                      │
│  ├── Acceptance criteria                             │
│  └── Design by Contract, reborn                      │
│  → See: layers/L3_CONTRACTS.md                       │
│                                                      │
│  ═══════════════════ HUMAN / AI BOUNDARY ══════════  │
│                                                      │
│  LAYER 4: GENERATION           [AI-owned]            │
│  ├── Code from contracts                             │
│  ├── Tests from contracts                            │
│  ├── Docs from code + contracts                      │
│  └── Traceability maintenance                        │
│  → See: layers/L4_GENERATION.md                      │
│                                                      │
│  LAYER 5: ALIGNMENT REVIEW     [AI-screened,         │
│  ├── Does contract match outcome? Human-judged]      │
│  ├── Does decomposition still hold?                  │
│  ├── Fagan Inspections for alignment                 │
│  └── AI handles correctness review                   │
│  → See: layers/L5_ALIGNMENT_REVIEW.md                │
│                                                      │
│  LAYER 6: MEASUREMENT          [AI-maintained,       │
│  ├── DORA metrics               Human-judged]        │
│  ├── Outcome metrics                                 │
│  ├── Traceability audits                             │
│  └── Feedback → Layer 1                              │
│  → See: layers/L6_MEASUREMENT.md                     │
│                                                      │
└──────────────────────────────────────────────────────┘
```

---

## The Three Rules

1. **Humans own VALUES, AUTHORITY, and ACCOUNTABILITY.** AI proposes outcomes, decompositions, and contracts. Humans approve, constrain, and correct. The irreducible human input is not intelligence — it's judgment about what matters, permission to proceed, and ownership of consequences.

2. **AI owns DERIVATION.** Implementation, docs, tests, traceability, metrics collection — anything that can be mechanically produced from the layers above. Humans spot-check, not author.

3. **Every artifact must fit in a context window** — this is the new "structured programming" constraint. It forces modularity, clear interfaces, and explicit dependencies. The models of the 80s demanded this for mathematical reasons. We demand it for practical ones. The result is the same: better software.

## What Is Irreducibly Human

When you strip away everything AI could do with sufficient access, you're left with:

| Element | Why It's Human |
|---|---|
| **Values** | "What matters more?" — can't be derived, must be stated |
| **Authority** | "Go/no-go" — someone must be accountable |
| **Tacit Context** | Org politics, unwritten rules, cultural norms |
| **Common Sense** | "A real person would never..." — requires lived experience |
| **Accountability** | "Who's responsible if this fails?" — can't be delegated to a machine |

None of these are intelligence tasks. The human role doesn't shrink because AI gets smarter. It shrinks when organizations **externalize context** (write down what's tacit), **rank values** (make trade-offs explicit), and **define delegation boundaries** (what AI can decide autonomously).

---

## What Was Actually Discarded vs. What Was Wrong

| Practice | Why Discarded | Actually Wrong? |
|---|---|---|
| Heavy documentation | Too expensive | **No** — now free |
| Formal specifications | Too slow | **No** — AI generates from them |
| Requirements traceability | Maintenance burden | **No** — AI maintains it |
| Fagan inspections | Human time cost | **No** — AI does tier 1 |
| Waterfall phases | Too rigid, late feedback | **Partially** — iteration matters |
| Spiral prototyping | Prototypes costly | **No** — now nearly free |
| Design by Contract | More work than code | **No** — contracts are now the input |

The only thing that was genuinely wrong was **assuming you could get alignment right upfront without iteration**. Everything else was just too expensive. It's not anymore.

---

## Summary

The 80s had the right instincts about rigor. The 90s-00s had the right instincts about iteration. The synthesis for the AI era is: **iterate on alignment (human), generate with rigor (machine), decompose to fit context windows (structural constraint).**
