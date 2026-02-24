# Mode 4: REFRESH

> **"Periodically re-evaluate mental models before they become wrong."**

## Purpose

Mental models decay. Assumptions that were true last month may not be true today. Markets shift, users surprise you, technology changes, teams turn over. REFRESH is the discipline of **deliberately updating** the mental models that drive alignment decisions.

Without REFRESH, you optimize for a world that no longer exists.

---

## Why Mental Models Decay

```
WHEN YOU DECIDED:                    WHAT MAY HAVE CHANGED:
──────────────────                   ───────────────────────
Outcome: "Reduce no-shows"          → Maybe no-shows dropped on their own
Decomposition: "3 units"            → Maybe a unit grew beyond context budget
Spec: "Quiet hours = no reminders"  → Maybe users want opt-in during quiet hours
Tech: "Use SMS provider X"          → Maybe provider X raised prices 300%
Team: "Team B owns delivery"        → Maybe Team B lost two engineers
Market: "Competitors don't do this" → Maybe a competitor just shipped it
```

### Decay Signals

```
LEADING INDICATORS (catch decay early):
├── Metrics plateauing despite shipping features → wrong lever
├── Specs getting more exceptions/edge cases → model doesn't fit reality
├── AI clarifying questions increasing → specs drifting from domain
├── Rework rate increasing → something changed that specs don't reflect
└── Team confusion increasing → shared model is fracturing

LAGGING INDICATORS (decay already caused damage):
├── Features shipped but outcome unchanged → built the wrong thing
├── Users using the product in unexpected ways → model was wrong
├── Production incidents in "stable" areas → assumptions violated
└── Team working on things not on the outcome map → alignment lost
```

---

## Refresh Cadences

### Daily: Context Refresh
```
What:  Re-read the outcome statement and active specs before starting work
Why:   Prevents "tunnel vision" — losing sight of WHY while focused on HOW
Time:  5 minutes
AI:    Generates a "morning brief" — what changed overnight, what's active
```

### Weekly: Assumption Check
```
What:  Review 3-5 key assumptions underlying current work
       "Is it still true that...?"
Why:   Catches slow drift before it compounds
Time:  30 minutes
AI:    Flags assumptions that haven't been validated recently
       Surfaces contradicting data if available
```

### Per-Milestone: Model Review
```
What:  Review decomposition, boundaries, and shared glossary
       "Does the shape still make sense?"
Why:   Units grow, requirements shift, teams change
Time:  1-2 hours
AI:    Reports context window utilization per unit
       Flags units approaching budget limits
       Identifies glossary terms used inconsistently
```

### Quarterly: Outcome Reassessment
```
What:  Re-evaluate the outcome itself
       "Is this still the right thing to optimize for?"
Why:   Markets shift, strategies evolve, learnings accumulate
Time:  Half-day
AI:    Trends outcome metrics over the quarter
       Compares to initial projections
       Surfaces new data that might change the outcome
```

---

## Refresh Techniques

### 1. Pre-Mortem
Before REFRESH, imagine the project failed. Ask: "What went wrong?"

```
"It's 3 months from now. We shipped everything but the outcome
didn't move. Why?"

Common answers:
- "We solved the wrong problem" → Outcome needs revision
- "The pieces don't fit together" → Decomposition needs revision
- "The specs missed a critical behavior" → Contracts need revision
- "The market changed" → Everything needs revision
```

### 2. Beginner's Mind
Explain the current state to someone new (or to AI with no prior context):

```
Prompt: "I'm going to describe a system we're building. With no
prior context, tell me what seems odd, contradictory, or risky."

Then load: outcome statement, decomposition map, key specs.

AI's fresh perspective catches assumptions that insiders are blind to.
```

### 3. Inversion
Ask the opposite question:

```
Instead of: "What should we build next?"
Ask:        "What should we STOP building?"

Instead of: "Is this spec correct?"
Ask:        "What would make this spec wrong?"

Instead of: "Are we on track?"
Ask:        "What evidence would tell us we're off track?"
```

### 4. Metrics Contradiction
Look for metrics that should move together but aren't:

```
EXPECTED:    Reminder open rate ↑ AND missed appointments ↓
OBSERVED:    Reminder open rate ↑ AND missed appointments flat
CONCLUSION:  Users open reminders but don't act → wrong solution
             (maybe they need reschedule options, not more reminders)
```

---

## Refresh and AI Context Windows

REFRESH has a direct context window implication: **stale context is wrong context.**

```
If your outcome statement was written 3 months ago and the market shifted,
every AI session that loads that statement is generating from wrong context.

If your glossary defines "appointment" one way but the team now uses it
differently, every AI session is working with wrong definitions.

If a unit's spec has 12 exception rules added since the original 5,
the spec may no longer fit cleanly in a context window.
```

REFRESH is **context hygiene**: keeping the documents that load into AI sessions accurate, current, and well-sized.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Daily context refresh | Re-reads outcome + active specs | Generates morning brief |
| Weekly assumption check | Validates "still true?" | Flags unvalidated assumptions, surfaces data |
| Milestone model review | Judges whether shape is still right | Reports utilization, drift, inconsistencies |
| Quarterly outcome reassessment | Decides whether to pivot | Trends metrics, compares projections |
| Pre-mortem | Generates failure scenarios | Stress-tests scenarios against current state |
| Beginner's mind | Initiates fresh review | Provides unbiased external perspective |
| Metrics contradiction | Interprets meaning | Identifies contradicting metric pairs |
| Context hygiene | Approves updates | Flags stale documents, suggests updates |
