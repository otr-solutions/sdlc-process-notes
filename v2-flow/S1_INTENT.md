# Station 1: INTENT

> **"Why does this work exist, and how will we know it succeeded?"**

## Role in the Flow

INTENT is the entry point. Nothing enters the system without a validated intent. It is the **most upstream station** — everything downstream pulls from it.

### Pull Relationship
SHAPE pulls from INTENT: "I need to decompose something. What outcome am I decomposing for?"

If INTENT has no validated items, the entire system is correctly idle. **Having nothing in INTENT is better than having the wrong thing.**

---

## WIP Limit: 2-3

An organization can meaningfully pursue 2-3 outcomes at a time. More than that and alignment fractures — people lose track of which outcome their work serves.

This is Goldratt applied to strategy: if you have 10 priorities, you have zero priorities.

---

## V-Model Pairing: Outcome Validation

```
INTENT ◄──────────────────────────────────────► OUTCOME VALIDATION

"Reduce no-shows to <10%"                      "No-show rate actually dropped?"
Defined before any work starts                  Measured after work ships
```

Outcome Validation is the **last** check in the entire system. It answers: "Did the intent materialize in reality?" The gap between intent and outcome validation is the **alignment lead time** — the most important metric in V2.

---

## Information Hiding (Parnas)

INTENT hides **strategic context** from downstream stations:

```
HIDDEN (stays in INTENT):              EXPOSED (interface to SHAPE):
───────────────────────                ──────────────────────────────
Why the board cares about this         Outcome statement
Competitive pressure driving it        Success metrics
Funding model behind it                Anti-goals
Political dynamics                     Constraints
Alternative outcomes considered        Priority ranking
```

Downstream stations don't need to know *why* this outcome was chosen. They need to know *what* the outcome is and *how it's measured*. This is Parnas's principle: define modules by what they hide, not what they do.

---

## PDCA Micro-Cycle

```
PLAN:  Draft outcome statement + metrics
       AI proposes from available data (financials, support tickets, usage analytics)

DO:    Socialize with stakeholders
       Test the outcome statement: "If we achieved this, would it matter?"

CHECK: Do stakeholders agree this is the right outcome?
       Is it measurable? Is it time-bound? Is it attributable?

ACT:   Refine or pivot. Repeat until stable.
       Stability test: same answer 3 conversations in a row = ready.
```

---

## Artifacts

### The Intent Card

One card per outcome. Fits on an index card (or in ~2K tokens).

```
OUTCOME:     Reduce missed appointment rate from 23% to under 10%

METRICS:
  Primary:   Missed appointment rate
  Leading:   Reminder open rate
  Guardrail: Booking volume doesn't decrease

ANTI-GOALS:
  - Not rebuilding the booking system
  - Not adding messaging/chat
  - Not changing the appointment data model

CONSTRAINTS:
  - Must work with existing SMS provider
  - HIPAA compliance required for health data
  - Ship within Q2

PRIORITY:    1 of 2 active outcomes this quarter
```

### Why It's Small

The intent card loads into **every AI context window** downstream. If it's too large, it steals context budget from the work. 2K tokens is the target — enough to orient, small enough to be overhead-free.

---

## Ownership

### Theory of Constraints View

INTENT is where humans are **most irreplaceable**. This is the constraint's constraint — the decision about *what to optimize for*. Goldratt says: exploit the constraint. That means:

- Don't waste human INTENT time on non-alignment activities
- Don't let INTENT decisions sit unvalidated
- Don't let INTENT get blocked by downstream stations
- Make INTENT decisions the highest-quality decisions in the system

### Who Does What

| Activity | Human | AI |
|---|---|---|
| Propose outcomes | Approves | Drafts from data analysis |
| Set metrics | Decides what matters | Suggests baselines and targets |
| Define anti-goals | Authors | Flags scope risks |
| Validate with stakeholders | Conducts conversations | Summarizes feedback |
| Monitor outcome (V-Model pair) | Judges "did it matter?" | Collects and trends data |

### Irreducible Human Input

```
├── Value ranking: "This outcome matters more than that one"
├── Risk appetite: "We'll bet on this even though it might fail"
├── Authority: "Go. We're doing this."
└── Accountability: "I own the result."
```

Everything else — data gathering, drafting, analysis, monitoring — is AI-generatable.
