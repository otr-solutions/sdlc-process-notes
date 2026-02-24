# Mode 3: OFFLOAD

> **"Delegate everything that doesn't require human judgment."**

## Purpose

Systematically identify every activity in the SDLC that AI can own, and **actually hand it over**. The goal isn't "AI assists humans" — it's "humans only do what AI cannot."

This is the inverse of FOCUS. FOCUS decides what to keep. OFFLOAD decides what to shed.

---

## The Offload Inventory

Every SDLC activity classified by offload potential:

### Fully Offloadable (AI owns, human doesn't see it)

```
├── Code generation from specs
├── Test generation from specs
├── Documentation generation from code + specs
├── Traceability maintenance
├── Dependency management and updates
├── Code formatting, linting, style enforcement
├── Security scanning (SAST, DAST, dependency audit)
├── Build and deployment pipeline execution
├── Metric collection and aggregation
├── Trend analysis and anomaly detection
├── Boilerplate and scaffolding
├── API reference documentation
├── Change impact analysis
└── Regression test execution
```

### Partially Offloadable (AI drafts, human reviews)

```
├── Contract/spec authoring (AI drafts, human corrects alignment)
├── Decomposition proposals (AI analyzes coupling, human decides boundaries)
├── Outcome proposals (AI analyzes data, human decides what matters)
├── Edge case enumeration (AI enumerates, human validates business relevance)
├── Integration testing (AI tests, human judges emergent behavior)
├── Metric interpretation (AI trends, human judges meaning)
└── Architecture decisions (AI proposes, human chooses based on strategy)
```

### Not Offloadable (human only)

```
├── Value ranking ("reliability > performance > cost")
├── Risk appetite ("we'll accept 20% chance of failure")
├── Authority ("approved, we're doing this")
├── Accountability ("I own this outcome")
├── Common sense ("a real user would never do this")
├── Organizational knowledge ("Team B can't handle this right now")
├── Strategic intuition ("the market is shifting toward X")
└── Ethical judgment ("this could harm users if...")
```

---

## The Offload Maturity Model

Teams don't offload everything at once. There's a progression:

### Level 1: AI as Typist
```
Human thinks → dictates to AI → AI types it
Status: Human does 95% of cognitive work
Risk: Low (AI is just a fast keyboard)
Value: Minimal (speed gain only)
```

### Level 2: AI as Drafter
```
Human outlines → AI drafts → Human edits heavily
Status: Human does 70% of cognitive work
Risk: Low (human reviews everything)
Value: Moderate (saves drafting time)
```

### Level 3: AI as Specialist
```
Human defines intent → AI produces artifacts → Human reviews for alignment
Status: Human does 30% of cognitive work
Risk: Medium (human must catch AI errors)
Value: High (focus shifts to alignment)
```

### Level 4: AI as Partner
```
Human sets values/goals → AI proposes + executes → Human judges outcomes
Status: Human does 15% of cognitive work
Risk: Medium-High (human must trust AI judgment)
Value: Very high (human attention fully on alignment)
```

### Level 5: AI as Autonomous Agent
```
Human sets constraints → AI operates within constraints → Human audits periodically
Status: Human does 5% of cognitive work
Risk: High (automation bias, loss of understanding)
Value: Maximum if trust is warranted
```

Most teams today are at Level 2-3. The V3 model targets Level 3-4.

---

## Offload Anti-Patterns

### 1. Rubber-Stamping
```
SYMPTOM:  Human "reviews" AI output in <30 seconds
CAUSE:    Automation bias — trusting AI because reviewing is boring
DANGER:   Alignment errors pass undetected
FIX:      Structured review checklists; random deep-dive audits
```

### 2. Shadow Work
```
SYMPTOM:  Human secretly redoes AI's work because "it's faster"
CAUSE:    AI output quality is poor, or human doesn't trust it
DANGER:   AI never improves; human stays overloaded
FIX:      Improve specs (AI quality follows spec quality); track rework rate
```

### 3. Offload Anxiety
```
SYMPTOM:  Human reviews everything AI produces, even commodity tasks
CAUSE:    Fear of AI errors; loss of control
DANGER:   Human attention wasted on extraneous load
FIX:      Graduated trust — start with low-risk offloads, build confidence
```

### 4. Over-Delegation
```
SYMPTOM:  Human delegates alignment decisions to AI
CAUSE:    Fatigue, time pressure, or misunderstanding of AI limitations
DANGER:   Misalignment goes undetected; wrong thing built perfectly
FIX:      Clear attention filter (FOCUS mode); non-delegatable list
```

---

## Offload Metrics

```
Offload Ratio:        % of SDLC activities handled by AI without human involvement
                      Target: 70-85%

Rework Rate:          % of AI outputs requiring human correction
                      Target: <15% (lower = specs are good; higher = specs need work)

Review Depth:         Average time human spends reviewing AI output
                      Target: varies by artifact type
                        - Contract review: 15-30 min (deep)
                        - Code review: 2-5 min (spot-check)
                        - Doc review: 0 min (skip unless flagged)

Attention Recapture:  Hours/week freed from extraneous work
                      Track to prove value of offloading

Automation Bias Rate: % of reviews with no changes (suspiciously high = rubber-stamping)
                      Target: 50-80% (some should genuinely need no changes)
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Classify offload potential | Reviews classifications | Proposes based on activity characteristics |
| Set offload boundaries | Decides delegation level | Operates within boundaries |
| Monitor offload quality | Spot-checks with deep-dives | Tracks rework rate, flags quality drops |
| Escalate from offload | Receives escalations | Flags when confidence is low |
| Expand offload scope | Approves when trust is earned | Proposes new offload candidates |
