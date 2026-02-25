# V4: Decision-Driven Development

> **Core Question: "What do we do when we don't know what to build?"**

## The Philosophical Difference

- V1 asks: "What's the right **structure**?"
- V2 asks: "What's the right **flow**?"
- V3 asks: "What deserves human **attention**?"
- V4 asks: "What's the riskiest **assumption**, and how do we test it cheaply?"

## Why This Dimension Matters

V1-V3 implicitly assume you **know what to build** — the challenge is structuring, flowing, and attending to it properly. But most software fails not because it was built badly, but because **the wrong bet was made**.

V4 treats every SDLC decision as a **bet under uncertainty** and optimizes for decision quality, not output quantity.

---

## Foundation Models

| Model | Era | Core Insight | V4 Application |
|---|---|---|---|
| Cynefin Framework (Snowden, 1999) | 90s | Different domains need different approaches | Classify each decision; apply the right method |
| OODA Loop (Boyd, 1976) | 70s | Observe-Orient-Decide-Act faster than the opponent | Alignment decisions as rapid orientation cycles |
| Real Options (finance, applied to SW) | 00s | Delay irreversible decisions; keep options open | Don't commit until you must; AI makes exploration cheap |
| Spiral Model (Boehm, 1986) | 80s | Risk-driven iteration; prototype the riskiest parts first | AI prototyping is free; test every risky assumption |
| Lean Startup (Ries, 2011) | 10s | Build-Measure-Learn; validated learning | Minimize time to validated alignment |

---

## The V4 Model: Four Decision Phases

```
┌─────────────────────────────────────────────────────┐
│             DECISION-DRIVEN DEVELOPMENT              │
│                                                      │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌───────────┐
│  │  SENSE   │──→│  FRAME   │──→│   BET    │──→│  LEARN    │
│  │          │   │          │   │          │   │           │
│  │ "What's  │   │ "What    │   │ "What's  │   │ "Were we  │
│  │  happening│   │  kind of │   │  the     │   │  right?"  │
│  │  out      │   │  problem │   │  cheapest│   │           │
│  │  there?"  │   │  is this"│   │  test?"  │   │           │
│  └──────────┘   └──────────┘   └──────────┘   └─────┬─────┘
│       ▲                                              │
│       └──────────────────────────────────────────────┘
│                    OODA CYCLE                         │
└─────────────────────────────────────────────────────┘
```

---

## The Cynefin Insight: Not All Decisions Are the Same

Snowden's Cynefin Framework classifies situations into domains. Each domain requires a fundamentally different approach:

```
┌─────────────────────┬─────────────────────┐
│      COMPLEX         │     COMPLICATED      │
│                      │                      │
│  Probe → Sense →     │  Sense → Analyze →   │
│  Respond             │  Respond             │
│                      │                      │
│  "We don't know      │  "We can figure      │
│   what will work.    │   this out with       │
│   Try things."       │   expertise."         │
│                      │                      │
├─────────────────────┼─────────────────────┤
│      CHAOTIC         │       CLEAR           │
│                      │                      │
│  Act → Sense →       │  Sense → Categorize → │
│  Respond             │  Respond             │
│                      │                      │
│  "Just act now.      │  "We know the         │
│   Stabilize first."  │   answer. Apply        │
│                      │   best practice."     │
└─────────────────────┴─────────────────────┘
```

### AI Changes the Cynefin Map

AI shifts many decisions between domains:

```
BEFORE AI:                              AFTER AI:
─────────                               ─────────
Code architecture → COMPLICATED          → CLEAR (AI applies patterns)
Test coverage → COMPLICATED              → CLEAR (AI generates from spec)
Edge case discovery → COMPLEX            → COMPLICATED (AI enumerates)
Market fit → COMPLEX                     → COMPLEX (still uncertain)
User values → COMPLEX                    → COMPLEX (still uncertain)
Production incident → CHAOTIC            → COMPLICATED (AI diagnoses faster)
```

What remains in the COMPLEX domain — where probing and experimentation are needed — is almost entirely **alignment decisions**: "Is this the right outcome? Will users behave this way? Is this boundary correct?"

---

## Real Options: Delay Commitment, Keep Options Open

Real Options theory from finance: **an option has value because it preserves future choice.** Don't exercise it until you must.

Applied to SDLC:

```
IRREVERSIBLE DECISIONS (delay these):    REVERSIBLE DECISIONS (make fast):
─────────────────────────────────────    ──────────────────────────────────
Architecture choices (database, lang)    Code implementation (regenerate)
Public API contracts                     Internal data structures
Team structure changes                   Configuration values
Outcome definition                       Test implementation
Decomposition boundaries                 Documentation wording
```

AI makes more decisions reversible by making regeneration cheap:

```
BEFORE AI: Choosing TypeScript vs. Go = 6 months of rewrite if wrong
AFTER AI:  Choosing TypeScript vs. Go = regenerate from same spec

→ Language choice moves from IRREVERSIBLE to REVERSIBLE
→ You can delay it, or change it cheaply later
```

### The Option Preservation Principle

```
At every decision point, ask:
1. Must I decide now, or can I delay?
2. If I decide, can I cheaply reverse it?
3. What information would I gain by waiting?
4. What's the cost of waiting?

If cost of waiting < value of information gained → WAIT
If cost of waiting > value of information gained → DECIDE
```

---

## V1-V4 Comparison

| Dimension | V1 (Structure) | V2 (Flow) | V3 (Cognitive) | V4 (Risk) |
|---|---|---|---|---|
| **Metaphor** | Architecture | Factory | Mental model | Decision landscape |
| **Organizing unit** | Layer | Station | Cognitive mode | Decision type |
| **Key question** | "Well-defined?" | "Flowing?" | "Well-spent?" | "Well-bet?" |
| **Optimization** | Completeness | Throughput | Attention | Decision quality |
| **Failure mode** | Over-specified | Unvalidated | Overwhelmed | Wrong bet |
| **AI role** | Generate | Execute | Handle extraneous | Make exploration cheap |
| **Best for** | Greenfield | Evolving products | Info overload | Uncertain domains |

→ See individual phase documents: [Sense](SENSE.md) · [Frame](FRAME.md) · [Bet](BET.md) · [Learn](LEARN.md)
