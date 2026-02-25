# Phase 2: FRAME

> **"What kind of problem is this — and what approach does it need?"**

## Purpose

Classify the decision you're facing. Different types of problems require fundamentally different approaches. Applying the wrong approach is worse than applying none at all.

This is Boyd's "Orient" — the most important step. **Orientation shapes everything downstream.** A misframed problem leads to well-executed wrong answers.

---

## The Cynefin Classification

For every decision surfaced by SENSE, classify it:

### Clear Domain
```
Characteristics: Cause and effect are obvious. Best practice exists.
Approach:        Sense → Categorize → Respond
AI role:         AI handles entirely. Apply the known pattern.

SDLC examples:
├── Code formatting decisions
├── Dependency version updates
├── Test generation from clear specs
├── Documentation updates
└── Deployment pipeline execution

Human involvement: NONE. This is fully offloaded.
```

### Complicated Domain
```
Characteristics: Cause and effect exist but require expertise to see.
Approach:        Sense → Analyze → Respond
AI role:         AI analyzes with domain expertise. Human validates.

SDLC examples:
├── Performance optimization (profiling → fix)
├── Security vulnerability remediation
├── Database schema design
├── API contract design
└── Integration architecture

Human involvement: REVIEW. AI proposes, human validates the analysis.
```

### Complex Domain
```
Characteristics: Cause and effect only visible in retrospect.
                 No right answer — only probe and respond.
Approach:        Probe → Sense → Respond
AI role:         AI generates probes cheaply. Human interprets results.

SDLC examples:
├── "Will users actually use this feature?"
├── "Is this the right outcome to pursue?"
├── "Where should we draw system boundaries?"
├── "Will this decomposition hold over time?"
└── "What will the market look like in 6 months?"

Human involvement: FULL. This is where alignment decisions live.
```

### Chaotic Domain
```
Characteristics: No cause and effect discernible. Crisis.
Approach:        Act → Sense → Respond
AI role:         AI stabilizes. Human directs.

SDLC examples:
├── Production outage with unknown cause
├── Security breach in progress
├── Key team member departure mid-sprint
└── Major dependency vulnerability (zero-day)

Human involvement: DIRECT. Act first, understand later.
```

---

## Why Framing Matters for AI Context Windows

The domain classification determines **what to load into context**:

```
CLEAR:       Load: spec + conventions
             Don't load: outcome, glossary (unnecessary overhead)
             AI needs: minimal context to apply known pattern

COMPLICATED: Load: spec + relevant precedents + domain expertise docs
             Don't load: broad outcome context
             AI needs: focused expertise context

COMPLEX:     Load: outcome + all relevant data + multiple perspectives
             Don't load: implementation details
             AI needs: maximum context for pattern recognition

CHAOTIC:     Load: current system state + immediate environment
             Don't load: anything non-urgent
             AI needs: diagnostic context only
```

**Different domains have different context packing strategies.** A one-size-fits-all approach wastes context budget.

---

## Framing Traps

### 1. Treating Complex as Complicated
```
SYMPTOM:  "Let's analyze our way to the right outcome"
REALITY:  There's no amount of analysis that reveals what users will do
FIX:      Probe (prototype, experiment) instead of analyze
```

### 2. Treating Complicated as Clear
```
SYMPTOM:  "Just use the standard approach"
REALITY:  This domain has nuances that the standard approach misses
FIX:      Get expertise involved; don't apply templates blindly
```

### 3. Treating Clear as Complex
```
SYMPTOM:  "Let's discuss the best code formatting approach"
REALITY:  Pick a standard and enforce it. This isn't worth alignment time.
FIX:      Offload to AI with a convention. Stop debating.
```

### 4. Staying in Complex When You've Learned Enough
```
SYMPTOM:  "Let's do one more experiment before committing"
REALITY:  You have enough signal. Continued probing is procrastination.
FIX:      Set decision criteria upfront: "We'll decide when we see X"
```

---

## The Frame-Then-Delegate Pattern

Once framed, the decision routes differently:

```
          SENSE
            │
         FRAME
         ╱    ╲     ╲      ╲
      CLEAR  COMPLI  COMPLEX  CHAOTIC
        │    CATED      │        │
      AI       │      Human    Human
     handles  AI      probes   acts
     fully  proposes  with AI  then
            Human     help     senses
            reviews
```

This is how FRAME connects to V3's FOCUS: **most decisions route away from human attention.** Only COMPLEX and CHAOTIC decisions require full human engagement.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Classify the domain | Validates AI's classification | Proposes classification from characteristics |
| Apply Clear approach | Nothing (offloaded) | Handles entirely |
| Apply Complicated approach | Reviews analysis | Provides expert analysis |
| Apply Complex approach | Leads probing, interprets results | Generates probes cheaply |
| Apply Chaotic approach | Acts and directs | Stabilizes and diagnoses |
| Detect framing traps | Catches misclassification | Flags patterns matching known traps |
