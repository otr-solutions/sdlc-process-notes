# V5: Trust-Graduated Development

> **Core Question: "How much autonomy does AI get — and under what conditions?"**

## The Philosophical Difference

- V1 asks: "What's the right **structure**?"
- V2 asks: "What's the right **flow**?"
- V3 asks: "What deserves human **attention**?"
- V4 asks: "What's the riskiest **assumption**?"
- V5 asks: "How much should I **delegate**, and what verification do I need?"

## Why This Dimension Matters

V1-V4 treat the human-AI relationship as a given: humans do alignment, AI does execution. But the **boundary is a spectrum**, not a binary. The real question every team faces daily is: *"Should I review this, or trust that AI got it right?"*

V5 models this as a **trust gradient** — a Principal-Agent relationship where the principal (human) delegates to the agent (AI) with varying levels of oversight based on risk, track record, and domain.

---

## Foundation Models

| Model | Era | Core Insight | V5 Application |
|---|---|---|---|
| Principal-Agent Theory (Jensen & Meckling, 1976) | 70s | Agents may not act in principal's best interest | AI's "interests" (training distribution) may not match yours |
| Trust but Verify (Reagan, arms control) | 80s | Delegate with checkpoints | Graduated oversight based on risk |
| Autonomy Levels (SAE driving automation, 0-5) | 10s | Standardized levels from no automation to full | Standardized levels for AI SDLC autonomy |
| Consumer-Driven Contracts (Fowler) | 00s | Downstream defines what it needs from upstream | Humans define what verification they need from AI |
| Delegation Poker (Management 3.0) | 10s | Explicit delegation levels per decision type | Map every SDLC activity to a trust level |

---

## The Trust Gradient: Five Levels

```
┌─────────────────────────────────────────────────────────┐
│                    TRUST GRADIENT                         │
│                                                          │
│  T1          T2          T3          T4          T5       │
│  ┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐   │
│  │HUMAN│    │AI    │    │AI    │    │AI    │    │AI    │   │
│  │ONLY │    │DRAFT │    │DECIDE│    │AUTO  │    │AUTON │   │
│  │     │    │HUMAN │    │HUMAN │    │HUMAN │    │HUMAN │   │
│  │     │    │DECIDE│    │REVIEW│    │AUDIT │    │NONE  │   │
│  └─────┘    └─────┘    └─────┘    └─────┘    └─────┘   │
│                                                          │
│  ◄─── More human control    More AI autonomy ───►        │
│  ◄─── Higher risk/novelty   Lower risk/routine ───►      │
│  ◄─── Less trust required   More trust required ───►     │
└─────────────────────────────────────────────────────────┘
```

### T1: Human Only
```
AI role:     None (or information gathering only)
Human role:  Full authorship and decision
Use when:    Novel strategic decisions, value trade-offs, authority needed
Examples:    Outcome definition, organizational priorities, ethical judgments
Verification: N/A (human is the source)
```

### T2: AI Drafts, Human Decides
```
AI role:     Proposes options, drafts artifacts
Human role:  Reviews thoroughly, makes the final call
Use when:    Complex domain, first-time decisions, high stakes
Examples:    Initial contract authoring, decomposition boundaries, spec review
Verification: Human reviews 100% of AI output before it takes effect
```

### T3: AI Decides, Human Reviews
```
AI role:     Makes the decision and executes
Human role:  Reviews after the fact, can override
Use when:    Complicated domain, established patterns, medium stakes
Examples:    Code generation, test generation, dependency updates
Verification: Human reviews output (but AI has already acted)
Rollback:    Easy — regenerate from spec if human disagrees
```

### T4: AI Autonomous with Guardrails
```
AI role:     Operates independently within defined constraints
Human role:  Sets guardrails, audits periodically
Use when:    Clear domain, proven track record, low stakes
Examples:    Formatting, linting, documentation updates, metric collection
Verification: Automated gates + periodic human audit (sampling)
Guardrails:  Defined boundaries that trigger escalation if crossed
```

### T5: Full AI Autonomy
```
AI role:     Operates without human involvement
Human role:  None (unless escalated)
Use when:    Commodity operations, fully understood, near-zero stakes
Examples:    Build pipeline, log aggregation, uptime monitoring
Verification: Automated only
Escalation:  AI escalates to T4/T3 if anomaly detected
```

---

## The Trust Map: SDLC Activities by Level

```
T1 (Human Only):
├── Outcome definition and prioritization
├── Value ranking ("reliability > performance")
├── Risk appetite decisions
├── Ethical/legal judgment calls
└── Accountability assignments

T2 (AI Drafts, Human Decides):
├── Contract/spec authoring
├── Decomposition boundary decisions
├── Architecture decisions (database, platform)
├── Integration review (emergent behavior)
└── Outcome reassessment

T3 (AI Decides, Human Reviews):
├── Implementation code generation
├── Test generation
├── Edge case enumeration
├── API documentation
├── Change impact analysis
└── Traceability updates

T4 (AI Autonomous + Guardrails):
├── Code formatting and linting
├── Security scanning
├── Dependency version management
├── Metric collection and dashboarding
├── Spec clarity scoring
└── Context window budget monitoring

T5 (Full AI Autonomy):
├── Build pipeline execution
├── Log aggregation
├── Uptime/health monitoring
├── Backup and restore procedures
└── Certificate renewal
```

---

## Trust Progression

Trust is **earned, not assumed**. Activities move up the gradient over time:

```
EARNING TRUST:
  New domain      → Start at T2 (AI drafts, human decides)
  After 5 reviews → Move to T3 (AI decides, human reviews)
  After 20 clean  → Move to T4 (AI autonomous + guardrails)
  runs

LOSING TRUST:
  One bad output   → Stay at current level, investigate
  Pattern of errors → Drop one level
  Critical failure  → Drop to T2 until root cause resolved
```

### Trust Metrics

```
Pass-Through Rate:   % of AI outputs accepted without changes
                     High (>90%) at current level → candidate for promotion

Override Rate:       % of AI decisions overridden by human
                     High (>20%) → trust level too high, demote

Escalation Rate:     % of T4/T5 items escalated to human
                     Healthy: 2-5%. Too low: guardrails too loose.

Trust Calibration:   Does the team's trust match AI's actual accuracy?
                     Over-trust: accepting bad output (automation bias)
                     Under-trust: reviewing things AI always gets right
```

---

## Context Window Implications

Trust level determines context loading strategy:

```
T1-T2:  Load full context — human needs all information to decide
T3:     Load spec + human gets a summary diff to review
T4:     Load spec only — human doesn't see output unless flagged
T5:     Minimal context — AI operates from conventions only
```

Higher trust = less context needed = faster processing = more throughput.

---

## Comparison

| Dimension | V1 | V2 | V3 | V4 | V5 |
|---|---|---|---|---|---|
| **Metaphor** | Architecture | Factory | Brain | Bet portfolio | Delegation chain |
| **Unit** | Layer | Station | Cognitive mode | Decision type | Trust level |
| **Question** | "Well-defined?" | "Flowing?" | "Well-spent?" | "Well-bet?" | "Well-delegated?" |
| **Failure** | Over-specified | Unvalidated | Overwhelmed | Wrong bet | Over/under-trust |

→ See trust level documents: [T1-Human](T1_HUMAN_ONLY.md) · [T2-Draft](T2_AI_DRAFTS.md) · [T3-Decide](T3_AI_DECIDES.md) · [T4-Auto](T4_AI_AUTONOMOUS.md) · [T5-Full](T5_FULL_AUTONOMY.md)
