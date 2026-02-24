# Mode 1: FOCUS

> **"What deserves human attention — and what doesn't?"**

## Purpose

Ruthlessly filter what reaches human consciousness. In a world where AI can generate unlimited artifacts, the danger isn't missing information — it's **drowning in it**.

Simon's warning (1971): *"A wealth of information creates a poverty of attention."* AI has created an information explosion. FOCUS is the defense.

---

## The Attention Filter

Every artifact, decision, and notification passes through a filter:

```
                    ┌──────────────┐
   All signals ───→ │ ATTENTION    │ ───→ Human reviews
   (unlimited)      │ FILTER       │      (7±2 items)
                    └──────────────┘
                          │
                          ▼
                    Handled by AI
                    (everything else)
```

### Filter Criteria

An item deserves human attention ONLY if it meets ALL of these:

```
1. IRREVERSIBLE   — The decision can't be cheaply undone
2. VALUES-LADEN   — It requires a judgment about what matters
3. NOVEL          — It hasn't been decided before (no precedent)
4. HIGH-IMPACT    — Getting it wrong would meaningfully affect the outcome
```

If any criterion is missing, AI handles it:

| Missing | Example | AI Action |
|---|---|---|
| Not irreversible | Code style choice | AI decides, human can override later |
| Not values-laden | Test implementation | AI generates from spec |
| Not novel | Same decision as last sprint | AI applies precedent |
| Not high-impact | Logging format | AI chooses convention |

---

## Attention Budget

Humans have a finite attention budget per day. Estimate conservatively:

```
Available focused attention:        ~4 hours/day (research consensus)
Alignment decisions per hour:       ~4-6 meaningful decisions
Daily alignment decision budget:    ~16-24 decisions

That means:
  Per sprint (10 days):             ~160-240 alignment decisions
  Per quarter:                      ~960-1440 alignment decisions

If your project requires more alignment decisions than this,
you need either:
  a) More humans (scaling the constraint)
  b) Better chunking (fewer, bigger decisions)
  c) More delegation to AI (moving decisions below the filter)
```

### Attention Allocation

```
Category                    % of Budget    Decisions/Day
──────────                  ──────────     ──────────────
Outcome alignment           20%            ~4
Decomposition/boundaries    15%            ~3
Contract review             30%            ~6
Integration judgment        15%            ~3
Metric interpretation       10%            ~2
Unexpected/novel issues     10%            ~2
                           ────           ────
                           100%           ~20
```

---

## Focus Practices

### 1. The Alignment-Only Standup
Traditional standup: "What did you do? What will you do? Blockers?"
V3 standup: **"What alignment decision do you need to make today?"**

If the answer is "none," the developer's work is fully offloaded — AI is generating, gates are passing, nothing needs human judgment. That's a **good** day, not a wasted one.

### 2. Decision Journaling
Track every alignment decision and its outcome. Over time, patterns emerge:

```
Decision: Separated scheduling from delivery
Type: Decomposition boundary
Time spent: 45 min
Confidence: High
Outcome: Correct — change rates differed as predicted
→ TEMPLATE THIS: similar domains should split by change rate
```

When a decision type gets templated enough times, it drops below the attention filter — AI applies the template.

### 3. Interrupt Shields
Protect focus time from extraneous interruptions:

```
AI-HANDLED (no human interrupt):     HUMAN-REQUIRED (interrupt allowed):
─────────────────────────────────     ───────────────────────────────────
Build failures (AI retries)           Repeated gate failures (spec issue)
Test failures (AI diagnoses)          New outcome proposal
Dependency updates                    Contract contradiction detected
Documentation drift                   Production incident
Metric collection                     Metric anomaly flagging
```

---

## Cognitive Load Mapping

Every item in the SDLC maps to a load type:

```
INTRINSIC (keep — this IS the job):
├── "Is this the right outcome?"
├── "Are these the right boundaries?"
├── "Does this contract capture our intent?"
├── "Do these metrics tell us what we need to know?"
└── "Should we pivot based on what we've learned?"

EXTRANEOUS (eliminate — this is waste):
├── Writing code that could be generated
├── Reviewing code for correctness (AI does this)
├── Maintaining docs manually
├── Running tests manually
├── Context-switching between unrelated projects
└── Meetings about status (dashboards exist)

GERMANE (maximize — this builds understanding):
├── Reviewing AI-generated contracts for alignment gaps
├── Discussing decomposition with the team
├── Analyzing why metrics moved (or didn't)
├── Post-incident learning
└── Updating mental models after user research
```

---

## Distributed Cognition (Hutchins)

Hutchins showed that cognition isn't just in heads — it's distributed across people, tools, and artifacts. In V3, the cognitive system includes:

```
HUMAN COGNITION:           AI COGNITION:              ARTIFACT COGNITION:
────────────────           ──────────────              ────────────────────
Values, judgment           Pattern matching            Contracts (encode rules)
Lived experience           Exhaustive enumeration      Glossary (shared meaning)
Common sense               Code generation             Decision logs (memory)
Strategic intuition        Consistency checking         Metrics dashboards (status)
                           Traceability maintenance     Outcome maps (purpose)
```

The "brain" of the project is the **entire system**, not any single participant. FOCUS is about directing the human portion of this distributed brain to where it adds the most value.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Decide what deserves attention | Manages the filter | Proposes filter criteria from history |
| Track attention budget | Monitors personal capacity | Tracks decision throughput metrics |
| Apply the attention filter | Makes the call on edge cases | Auto-handles everything below threshold |
| Interrupt shielding | Sets boundaries | Handles interrupts that don't require humans |
| Decision journaling | Records reasoning and confidence | Suggests templates from patterns |
