# V2: Flow-Aligned Development

> V1 asked: "What's the right **structure**?" (layers, contracts, decomposition)
> V2 asks: "What's the right **flow**?" (constraints, pull, cycles)

## The Philosophical Difference

V1 is **architectural** — it defines a static structure (six layers) and populates each one. It's top-down, spec-first, and implicitly sequential even when iterative. Its ancestry is Structured Analysis and Design by Contract.

V2 is **flow-based** — it treats software development as a production system where **alignment is the constraint**, not code. It's pull-based, cycle-driven, and optimizes for throughput of *validated alignment*, not throughput of features. Its ancestry is Theory of Constraints, Deming's PDCA, Kanban, and the V-Model.

Both achieve the same three goals:
1. **Managing alignment** between intent and implementation
2. **Delivering value**, not just shipping features
3. **Working within AI context window constraints**

But they get there differently.

---

## Foundation Models

| Model | Era | Core Insight | V2 Application |
|---|---|---|---|
| Theory of Constraints (Goldratt, 1984) | 80s | Find the bottleneck; everything else is subordinate | The bottleneck is alignment, not code production |
| PDCA / Deming Cycle (Shewhart/Deming) | 50s-80s | Plan-Do-Check-Act in tight cycles | Micro-cycles of alignment verification |
| V-Model (German federal standard, 1990s) | 90s | Every abstraction level has a symmetric validation | Every alignment decision has a corresponding verification |
| Information Hiding (Parnas, 1972) | 70s | Modules hide design decisions behind interfaces | Context windows are natural information-hiding boundaries |
| Kanban (Ohno/Toyota, 1950s-80s) | 80s | Pull-based flow, limit work in progress | Pull alignment decisions; limit WIP on human judgment |
| Feature-Driven Development (Palmer/Felsing, 2002) | 00s | Feature-shaped units of work, model-first | Context-window-shaped features, outcome-first |
| Model-Driven Architecture (OMG, 2001) | 00s | Platform-independent models → platform-specific code | Intent-models → AI-generated implementations |

---

## The Core Argument: Alignment Is the Constraint

### Theory of Constraints (Goldratt)

Goldratt's insight: **every system has exactly one constraint (bottleneck). Optimizing anything other than the constraint is waste.**

In the 90s, the constraint was **code production**. Developers were expensive. Code took time. So we optimized for developer productivity: better languages, better tools, better frameworks.

In the AI era, the constraint has shifted:

```
ERA         CONSTRAINT              RESULT
────────    ─────────────────       ──────────────────────────
1990s       Code production         Optimize: languages, tools, IDEs
2000s       Integration/deploy      Optimize: CI/CD, DevOps
2010s       Feedback speed          Optimize: Agile, microservices
2020s+      ALIGNMENT               Optimize: ??? ← we are here
```

Code is nearly free. Deployment is automated. Feedback loops are fast. **The bottleneck is now: "Are we building the right thing?"** Every hour spent writing code that's misaligned is waste. Every feature that doesn't connect to an outcome is inventory.

### Goldratt's Five Focusing Steps, Applied

```
1. IDENTIFY the constraint     → Alignment between intent and implementation
2. EXPLOIT the constraint      → Make every alignment decision as high-quality as possible
3. SUBORDINATE everything else → All other activities (coding, testing, docs) serve alignment
4. ELEVATE the constraint      → Invest in better alignment tools and practices
5. REPEAT                      → If alignment is no longer the constraint, find the new one
```

---

## The V2 Model: Alignment Flow

Instead of six layers, V2 has **four stations** in a pull-based flow, with a validation cycle at each station. Work flows through the system only when alignment is verified.

```
┌─────────────────────────────────────────────────────────────┐
│                    ALIGNMENT FLOW                            │
│                                                              │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌────────┐ │
│  │  INTENT  │───→│  SHAPE   │───→│  SPEC    │───→│  MAKE  │ │
│  │          │    │          │    │          │    │        │ │
│  │ "Why"    │    │ "What    │    │ "What    │    │ "Build │ │
│  │          │    │  pieces" │    │  exactly" │    │  it"   │ │
│  └────┬─────┘    └────┬─────┘    └────┬─────┘    └───┬────┘ │
│       │               │               │              │      │
│       ▼               ▼               ▼              ▼      │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌────────┐ │
│  │ VALIDATE │    │ VALIDATE │    │ VALIDATE │    │VALIDATE│ │
│  │ "Still   │    │ "Pieces  │    │ "Spec    │    │"Works?"│ │
│  │  right?" │    │  right?" │    │  right?" │    │        │ │
│  └──────────┘    └──────────┘    └──────────┘    └────────┘ │
│                                                              │
│  ◄──────────── PULL (downstream pulls from upstream) ──────► │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### How Pull Works

Traditional (push): "We defined the outcome, now decompose, now specify, now build."
V2 (pull): **"We need to build X. What spec does it need? What shape supports that spec? What intent justifies that shape?"**

Pull means:
- Nothing enters a station without demand from downstream
- No spec is written without a build need
- No shape is defined without a spec need
- No intent is articulated without a shape need

This prevents the classic failure: beautifully documented outcomes that nobody builds against.

### WIP Limits (from Kanban)

The critical insight: **limit the number of alignment decisions in progress at any time.**

```
Station     WIP Limit    Why
──────      ─────────    ─────────────────────────────────────
INTENT      2-3          An org can only meaningfully pursue 2-3 outcomes
SHAPE       3-5          Each outcome decomposes into a few units
SPEC        5-8          Specs ahead of build, but not too far ahead
MAKE        No limit     AI can generate in parallel; this isn't the constraint
```

WIP limits exist on the **human judgment** stations. The MAKE station has no limit because it's AI-driven — it's not the constraint.

---

## Station 1: INTENT

### What Happens Here
Articulate **why** this work exists and **how you'll know it succeeded**.

### V-Model Pairing
Intent pairs with **Outcome Validation** — the last check in the cycle. Did the intent actually materialize in the real world?

```
         INTENT ◄──────────────────────────► OUTCOME VALIDATION
         "Reduce no-shows to <10%"           "Did no-shows actually drop?"
```

### Information Hiding Principle (Parnas)
Intent **hides strategic context** from downstream stations. The SHAPE station doesn't need to know *why* reducing no-shows matters to the board. It only needs to know the outcome statement and success metrics. This is information hiding — downstream stations see the interface (outcome + metrics), not the implementation (strategy, politics, funding).

### PDCA Micro-Cycle
```
PLAN:  Draft outcome statement + metrics
DO:    Socialize with stakeholders, test assumptions
CHECK: Do stakeholders agree this is the right outcome?
ACT:   Refine or pivot. Repeat until stable.
```

### Artifacts (same goals as V1 Layer 1, different framing)
- **Outcome ticket**: Single sentence + 3-5 metrics + anti-goals
- Sized to fit in ~2K tokens (loads into every downstream context)

---

## Station 2: SHAPE

### What Happens Here
Decompose the problem into **context-window-sized units** with clear interfaces.

### V-Model Pairing
Shape pairs with **Integration Validation** — when units connect, does the combined shape still serve the intent?

```
              SHAPE ◄────────────────► INTEGRATION VALIDATION
              "3 units, these boundaries"   "Do they compose correctly?"
```

### Information Hiding Principle (Parnas)
Each unit **hides its internals** from other units. The boundary IS the information-hiding boundary. Parnas's original insight was that modules should be defined by "what they hide," not "what they do." In V2, a unit hides:
- Its implementation strategy
- Its internal data structures
- Its AI generation approach
- Its context window contents

Other units see only the interface contract.

### Feature-Driven Development Mapping
FDD's "feature" concept maps directly: each unit is a **feature-shaped chunk** that delivers recognizable value, can be built independently, and has a clear owner.

```
FDD Feature Template → V2 Unit Template:
<action> <result> <object>

"Schedule reminder for appointment"     → reminder-scheduling unit
"Deliver notification to user"          → notification-delivery unit
"Reschedule appointment from reminder"  → reschedule-flow unit
```

### PDCA Micro-Cycle
```
PLAN:  Propose unit boundaries based on coupling analysis
DO:    Map dependencies, estimate context budgets
CHECK: Does each unit fit in a context window? Is it cohesive?
ACT:   Split oversized units, merge over-fragmented ones. Repeat.
```

### Context Window Budget
```
Unit context budget:
├── Shared context (outcome, glossary, schemas): ~10K tokens
├── Unit's own spec: ~5-10K tokens
├── Unit's implementation: ~15-25K tokens
├── Room for AI reasoning: remaining ~40%
└── TOTAL must fit: ≤ context window size
```

---

## Station 3: SPEC

### What Happens Here
Define **exactly** what each unit does — preconditions, postconditions, invariants, edge cases. This is where Model-Driven Architecture enters.

### Model-Driven Architecture Mapping
MDA defines three model levels:

```
MDA Level                    V2 Equivalent
─────────                    ─────────────
CIM (Computation-Independent) → INTENT (business outcome, no tech)
PIM (Platform-Independent)    → SPEC (what the unit does, no how)
PSM (Platform-Specific)       → MAKE (TypeScript, PostgreSQL, AWS)
```

The spec IS the Platform-Independent Model. It says what must happen without committing to how. AI transforms PIM → PSM automatically.

### V-Model Pairing
Spec pairs with **Contract Validation** — does the generated code actually satisfy the spec?

```
                   SPEC ◄──────────► CONTRACT VALIDATION
                   "These rules, these invariants"   "Code satisfies all rules?"
```

This is where the V-Model's symmetry shines. Every abstraction level has a corresponding test level:

```
INTENT  ◄─────────────────────────────────────► OUTCOME VALIDATION
  SHAPE  ◄───────────────────────────────► INTEGRATION VALIDATION
    SPEC  ◄─────────────────────────► CONTRACT VALIDATION
      MAKE  ◄───────────────────► UNIT VALIDATION
```

### PDCA Micro-Cycle
```
PLAN:  AI drafts spec from intent + shape
DO:    Human reviews, corrects, adds domain knowledge
CHECK: Can AI generate without clarifying questions? (clarity test)
ACT:   Refine ambiguous sections. Repeat until AI asks zero questions.
```

### Spec Clarity Metric
A spec's readiness is measurable: **feed it to AI and count the clarifying questions.** Zero questions = ready for MAKE. This is a concrete, testable quality gate.

---

## Station 4: MAKE

### What Happens Here
AI generates all derivable artifacts: code, tests, docs, traceability.

### V-Model Pairing
Make pairs with **Unit Validation** — does each generated artifact work in isolation?

### Pull Trigger
MAKE only starts when SPEC is validated. It **pulls** a spec from the upstream queue. If no validated specs exist, MAKE is idle — and that's correct. The constraint is upstream.

### PDCA Micro-Cycle
```
PLAN:  Load context (shared + unit spec)
DO:    AI generates implementation + tests + docs
CHECK: Automated gates (compile, test, coverage, security)
ACT:   If gates fail, AI regenerates. If repeated failure, spec needs refinement.
```

---

## The Validation Spine (V-Model)

The V-Model's key insight: **validation should be symmetric with abstraction**. Every level of "going down" into detail has a corresponding level of "coming back up" to verify.

```
ABSTRACTION SIDE                              VALIDATION SIDE
────────────────                              ────────────────

INTENT                                        OUTCOME VALIDATION
  "Reduce no-shows <10%"          ◄──────►    "No-show rate actually dropped?"
    │                                              ▲
    ▼                                              │
SHAPE                                         INTEGRATION VALIDATION
  "3 units, these boundaries"     ◄──────►    "Units compose correctly?"
    │                                              ▲
    ▼                                              │
SPEC                                          CONTRACT VALIDATION
  "These rules, these invariants" ◄──────►    "Code satisfies all rules?"
    │                                              ▲
    ▼                                              │
MAKE                                          UNIT VALIDATION
  "Generated code"                ◄──────►    "Compiles, tests pass?"
```

**Who validates at each level:**

| Level | Validator | Why |
|---|---|---|
| Unit Validation | AI (automated gates) | Mechanical — does it work? |
| Contract Validation | AI (spec verification) | Formal — does it match the spec? |
| Integration Validation | AI-screened, Human-judged | Emergent behavior needs human judgment |
| Outcome Validation | Human-judged (with AI data) | "Did it matter?" is a values question |

The bottom two are AI-owned. The top two require human judgment. The V-Model makes this symmetry explicit.

---

## WIP Limits and Flow Metrics

### Kanban Board

```
┌──────────┬──────────┬──────────┬──────────┬──────────┐
│  INTENT  │  SHAPE   │   SPEC   │   MAKE   │   DONE   │
│  WIP: 3  │  WIP: 5  │  WIP: 8  │ WIP: ∞   │          │
├──────────┼──────────┼──────────┼──────────┼──────────┤
│ Outcome A│ Unit A1  │ Spec A1a │ ████████ │ ████████ │
│ Outcome B│ Unit A2  │ Spec A2a │ ████████ │          │
│          │ Unit B1  │ Spec B1a │          │          │
│          │          │ Spec A1b │          │          │
└──────────┴──────────┴──────────┴──────────┴──────────┘

████████ = AI is working (no human bottleneck)
```

### Flow Metrics

```
Alignment Lead Time:    Time from INTENT to OUTCOME VALIDATION
                        (the full cycle)

Alignment Throughput:   Number of validated alignment decisions per week
                        (not features shipped — alignment decisions confirmed)

Alignment WIP:          Number of unvalidated alignment decisions in flight
                        (leading indicator of future problems)

Constraint Utilization: % of time the human-judgment bottleneck is actively
                        making alignment decisions (vs. waiting, context-switching,
                        or doing AI's job)
```

### The Key Metric: Constraint Utilization

If alignment is the constraint, the most important metric is: **what percentage of human time is spent on alignment decisions vs. everything else?**

```
BAD:  Human spends 20% on alignment, 80% on code/tests/docs/meetings
      → Constraint is underutilized. AI should handle the 80%.

GOOD: Human spends 70% on alignment, 30% on unavoidable coordination
      → Constraint is well-exploited. System is optimized.
```

---

## V1 vs. V2 Comparison

| Dimension | V1 (Layers) | V2 (Flow) |
|---|---|---|
| **Metaphor** | Architecture (static structure) | Production system (dynamic flow) |
| **Primary models** | Structured Analysis, Design by Contract, Cleanroom | Theory of Constraints, V-Model, Kanban, PDCA |
| **Organizing principle** | Six layers, top-down | Four stations, pull-based |
| **Key question** | "Is each layer well-defined?" | "Is alignment flowing without blockage?" |
| **Human role** | Author/approve at each layer | Unblock the constraint (alignment decisions) |
| **AI role** | Generate from specs | Execute everything that isn't the constraint |
| **Quality approach** | Contract quality checklist | V-Model symmetric validation |
| **Progress metric** | Layers completed | Alignment throughput |
| **Risk model** | Contract gaps, decomposition errors | WIP overflow, constraint starvation |
| **Failure mode** | Over-specified but misaligned | Fast but unvalidated |
| **Best for** | Greenfield, well-understood domains | Evolving products, uncertain domains |

---

## When to Use Which

**Use V1 (Layers)** when:
- Building a new system from scratch
- The domain is well-understood
- You want rigorous traceability
- Regulatory/compliance requirements demand documentation
- The team is new to AI-assisted development

**Use V2 (Flow)** when:
- Evolving an existing product
- Requirements are uncertain or changing rapidly
- Speed of validated learning matters more than completeness
- The team is experienced with AI tools
- You need to optimize human attention (the real scarce resource)

**Use both** when:
- V1 provides the structural framework (what artifacts exist)
- V2 provides the process framework (how work flows through)
- They're complementary, not competing

---

## Summary

V1 says: **"Define the right structure, then fill it in."**
V2 says: **"Find the bottleneck, exploit it, let everything else flow."**

Both agree on the fundamentals:
- The bottleneck is alignment, not code
- AI context windows are a design constraint that forces good modularity
- Humans own values, authority, and accountability
- Everything derivable should be derived by AI

They disagree on emphasis:
- V1 emphasizes **completeness** — every layer well-defined before moving on
- V2 emphasizes **flow** — validated alignment decisions moving through the system as fast as possible

The best teams will use V1's structure with V2's dynamics.
