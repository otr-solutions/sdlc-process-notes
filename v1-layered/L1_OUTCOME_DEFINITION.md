# Layer 1: Outcome Definition

> **Ownership: AI-Proposed, Human-Approved**

## Purpose

Answer the question: **"What changes in the world if we succeed?"** before anyone writes a line of code or a spec. This layer exists to prevent the most expensive failure mode in software: building the wrong thing perfectly.

---

## Artifacts

### Outcome Statement

A single sentence that describes the measurable change. Not a feature. Not a deliverable. A *change in state*.

```
BAD:  "Build a notification system"          ← that's output
BAD:  "Users can configure notifications"    ← that's a feature
GOOD: "Reduce missed appointment rate from 23% to under 10%"  ← that's an outcome
```

### Opportunity Solution Tree

Borrowed from Teresa Torres. Maps the outcome to the opportunities (user pain points) that, if addressed, would drive the outcome. Then maps opportunities to potential solutions.

```
Outcome: Reduce missed appointments from 23% to <10%
├── Opportunity: Users forget appointments exist
│   ├── Solution: SMS reminder 24hrs before
│   ├── Solution: Calendar integration
│   └── Solution: Push notification morning-of
├── Opportunity: Users can't easily reschedule
│   ├── Solution: One-tap reschedule from reminder
│   └── Solution: Self-service reschedule portal
└── Opportunity: Users book too far in advance
    └── Solution: Limit booking window to 2 weeks
```

This tree is the **alignment anchor** for everything downstream. When a developer asks "should I build X?" the answer is: "Does it connect to a node on this tree?"

### Success Metrics

How you'll know the outcome is happening. Must be:
- **Measurable** before the project ships (leading indicators)
- **Attributable** to the work being done (not vanity metrics)
- **Time-bound** (when do we evaluate?)

```
Primary:   Missed appointment rate (currently 23%, target <10%)
Leading:   Reminder open rate (proxy for engagement)
Guardrail: Appointment booking volume doesn't decrease
           (we're not "solving" missed appointments by discouraging booking)
```

### Anti-Goals

Explicitly state what you are NOT trying to do. This prevents scope creep and keeps AI-generated solutions bounded.

```
Anti-goals:
- We are NOT rebuilding the booking system
- We are NOT adding a chat/messaging feature
- We are NOT changing the appointment data model
```

---

## Ownership Analysis

### What's Actually Happening When a Human Does This

```
1. Intake business context    → "We're losing money on missed appointments"
2. Identify the lever         → "If we reduce no-shows, revenue recovers"
3. Set a target               → "From 23% to under 10%"
4. Prioritize against others  → "This matters more than the redesign"
5. Define what NOT to do      → "Don't touch the booking system"
```

### What Would AI Need to Own This

| Sub-task | Barrier | Type |
|---|---|---|
| Intake business context | AI needs access to financials, customer data, support tickets, market signals, competitor moves | **Access problem** — solvable by providing data |
| Identify the lever | AI needs causal reasoning: "missed appointments → revenue loss" is a causal claim, not just correlation | **Capability gap** — AI can do this today with good data, but humans don't trust it yet |
| Set a target | Requires understanding of what's *feasible* given resources, time, and organizational capacity | **Judgment under uncertainty** — AI can model scenarios, but "what to bet on" is a values question |
| Prioritize against alternatives | Requires understanding organizational strategy, politics, appetite for risk, CEO's vision | **Unformalized context** — lives in hallway conversations, board meetings, gut feelings |
| Define anti-goals | Requires knowing what's politically/strategically off-limits, what's sacred, what's fragile | **Tacit knowledge** — nobody writes this down because it's "obvious" to insiders |

### The Honest Assessment

**Most of Layer 1 is not fundamentally human.** It's human because:

1. **The context isn't externalized.** Strategy lives in people's heads, in Slack DMs, in board decks that AI doesn't have access to. If you gave AI full access to business data, customer research, strategic plans, and financial models — it could *propose* outcomes.

2. **The values aren't formalized.** "We care more about customer retention than revenue growth this quarter" is a value statement. AI can optimize *for* a value. It can't *choose* a value. But most organizations don't have that many core values — they could be enumerated.

3. **Accountability is human.** Even if AI proposed the perfect outcome, someone has to say "yes, we're doing this." That's not intelligence — it's **authority**. Boards, regulators, and customers hold *people* accountable.

---

## Irreducible Human Input

```
IRREDUCIBLE:
├── Value ranking: "Retention > revenue > growth this quarter"
├── Risk appetite: "We'll accept 20% chance of failure on this bet"
├── Authority: "Approved. We're doing this."
└── Accountability: "I own the result."

AI-GENERATABLE:
├── Opportunity identification from data
├── Outcome proposals with confidence intervals
├── Target setting based on benchmarks and capacity
├── Anti-goal suggestions based on dependency analysis
└── Opportunity solution tree drafts
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define the outcome | Approves | Proposes from data |
| Build opportunity tree | Reviews & refines | Drafts from customer/user data |
| Set success metrics | Decides what matters | Pulls baseline data, suggests targets |
| Define anti-goals | Authors | Flags scope risks, suggests boundaries |
| Prioritize against alternatives | Decides | Models trade-offs and scenarios |

---

## Context Window Consideration

This entire layer should produce **one document, under 2 pages**, that gets loaded into every AI context downstream. It's the "system prompt" for your project.
