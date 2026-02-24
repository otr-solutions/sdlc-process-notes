# Phase 2: TRANSMIT

> **"Move signal through channels without degradation."**

## Purpose

Once intent is encoded into artifacts, those artifacts must travel: between teams, into AI context windows, through code generation, into production. Each transmission is a potential corruption point. TRANSMIT designs channels to minimize signal loss.

---

## Channel Analysis

### Channel 1: Artifact → AI Context Window

```
NOISE SOURCES:
├── Partial loading: Not all relevant artifacts fit in context
├── Irrelevant loading: Noise artifacts steal bandwidth from signal
├── Ordering effects: AI weights early context more than late
└── Stale artifacts: Outdated content transmits wrong signal

MITIGATION:
├── Context packing strategy (load signal-dense artifacts first)
├── Progressive disclosure (summary → detail on demand)
├── Freshness checks (flag artifacts older than N days)
└── Dedicated "context manifest" listing what to load for each task
```

### Channel 2: AI Context → Generated Code

```
NOISE SOURCES:
├── Training bias: AI defaults to patterns from training, not spec
├── Hallucination: AI generates plausible but incorrect code
├── Spec ambiguity: Unclear rules → AI fills gaps with assumptions
└── Context overflow: Too much loaded → AI loses coherence

MITIGATION:
├── Spec clarity test (zero-question standard)
├── Contract-first test generation (tests as error detection)
├── Regeneration over editing (prevent accumulated drift)
└── Context budget discipline (≤60% utilization)
```

### Channel 3: Human → Human (meetings, docs, chat)

```
NOISE SOURCES:
├── Paraphrasing: Each retelling loses nuance
├── Selective hearing: People hear what confirms their mental model
├── Context loss: New team members lack historical context
└── Medium distortion: Slack messages lack tone; emails lack urgency

MITIGATION:
├── Shared artifacts as source of truth (not conversations)
├── AI-generated meeting summaries (consistent encoding)
├── Decision records (decisions don't depend on memory)
└── Onboarding context packages (AI-assembled for new members)
```

### Channel 4: Code → Production → User Experience

```
NOISE SOURCES:
├── Configuration drift: Prod differs from dev/staging
├── Data differences: Real data exposes unconsidered cases
├── Scale effects: Behavior changes under load
└── Environment variance: Browser, device, network conditions

MITIGATION:
├── Infrastructure as Code (reduce config drift)
├── Feature flags (controlled exposure)
├── Observability (detect when signal doesn't reach users)
└── Synthetic testing in production (verify end-to-end signal)
```

---

## The Context Manifest

For each unit/task, define exactly what loads into the AI context window:

```markdown
## Context Manifest: reminder-scheduling generation

LOAD ORDER (signal-dense first):
1. outcome-definition.md (2K tokens) — WHY we're doing this
2. glossary.md (2K tokens) — WHAT words mean
3. appointment-schema.md (3K tokens) — DATA structures
4. reminder-scheduling.contract.md (8K tokens) — WHAT to build
5. Adjacent interfaces only: ReminderEvent schema (500 tokens)

DO NOT LOAD:
- notification-delivery implementation
- reschedule-flow spec
- Historical decision records (unless directly referenced)
- Any other unit's code

TOTAL: ~15.5K tokens loaded / ~50K available for generation
```

The manifest is **transmission engineering**: designing the channel to carry maximum signal.

---

## Transmission Metrics

```
Signal-to-Noise Ratio:   Tokens of relevant context / Total tokens loaded
                         Target: >70% (most loaded context is signal)

Transmission Loss:       What % of spec rules are reflected in generated code?
                         Measure: automated spec compliance check
                         Target: 100% (any loss is a bug)

Channel Utilization:     Context tokens used / Context tokens available
                         Target: 50-65% (enough signal, enough reasoning room)

Staleness Index:         Average age of artifacts in context
                         Flag: any artifact >30 days without review
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Design context manifests | Validates what's included/excluded | Proposes optimal loading order |
| Monitor channel quality | Reviews transmission metrics | Measures signal-to-noise, staleness |
| Maintain artifact freshness | Approves updates | Flags stale content, suggests refreshes |
| Reduce transmission noise | Decides what to simplify/remove | Identifies low-signal-density artifacts |
| Onboard new team members | Welcomes, provides human context | Assembles context package from artifacts |
