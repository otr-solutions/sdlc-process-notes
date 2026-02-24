# Phase 3: BET

> **"What's the cheapest way to test the riskiest assumption?"**

## Purpose

Make decisions explicitly as **bets** — with stated assumptions, expected outcomes, and decision criteria. This is where Real Options theory and the Spiral Model converge: **delay commitment until you must, and test the riskiest thing first.**

---

## Every Decision Is a Bet

Strip away the jargon and every SDLC decision is a bet:

```
"We'll build a reminder system"
= "We BET that reminders will reduce missed appointments"

"We'll separate scheduling from delivery"
= "We BET that these concerns change at different rates"

"We'll use SMS as the primary channel"
= "We BET that users respond to SMS more than push/email"
```

Making the bet explicit forces clarity:

### The Bet Card

```
BET:          SMS reminders will reduce no-shows
ASSUMPTION:   Users read SMS within 1 hour of receipt
EVIDENCE:     Industry data shows 95% SMS open rate within 5 min
RISK:         Users may have notifications silenced / wrong numbers
TEST:         Send SMS to 500 users for 2 weeks, measure open+action rate
COST OF TEST: ~$50 in SMS fees + AI generates the feature in 1 day
DECISION CRITERIA:
  - If open rate >80% AND action rate >30% → COMMIT to SMS-first
  - If open rate >80% AND action rate <10% → Reminders work but SMS isn't enough
  - If open rate <50% → Wrong channel, test push notifications
REVERSIBILITY: HIGH (can switch channels by updating spec + regenerating)
```

---

## The Risk Stack

Rank assumptions by **cost of being wrong × probability of being wrong**:

```
ASSUMPTION                         WRONG COST    WRONG PROB    RISK SCORE
────────────                       ──────────    ──────────    ──────────
"Reminders will reduce no-shows"   Very High     Medium        ████████ TEST FIRST
"SMS is the best channel"          Medium        Medium        █████ TEST SECOND
"24hr is the right lead time"      Low           High          ████ TEST THIRD
"Our SMS provider is reliable"     Medium        Low           ██ MONITOR
"TypeScript is the right language"  Low          Low           █ IGNORE (reversible)
```

### The Spiral Principle

Boehm's insight: **address the highest risks first.** Each cycle:
1. Identify the riskiest unvalidated assumption
2. Design the cheapest test
3. Run the test
4. Update the risk stack based on results
5. Repeat

AI makes this dramatically cheaper:

```
BEFORE AI:                              AFTER AI:
─────────                               ─────────
Prototype = weeks of development        Prototype = hours of generation
Test = manual QA + staged rollout       Test = AI generates + automated gates
Cost of being wrong = months of waste   Cost of being wrong = regenerate from spec
```

---

## Bet Types

### 1. Outcome Bets (highest stakes)
```
"This outcome is worth pursuing"
Test: Customer interviews, market data analysis, pilot programs
Cost: Time (human attention), not code
Decision: Human only — this is a values judgment
```

### 2. Architecture Bets (medium stakes, decreasing)
```
"This decomposition will hold"
Test: Build two units, connect them, observe the seam
Cost: AI generates both units cheaply; test the interface
Decision: Human validates; AI tests compatibility
```

### 3. Specification Bets (medium stakes)
```
"These rules capture user expectations"
Test: Generate prototype from spec, show to users
Cost: AI generates prototype in minutes
Decision: Human judges user reaction
```

### 4. Implementation Bets (lowest stakes)
```
"This technical approach will perform"
Test: AI generates, benchmark, compare alternatives
Cost: Nearly zero — generate multiple alternatives and benchmark
Decision: AI-decidable (measurable outcome)
```

### AI Makes Lower-Stakes Bets Free

```
BEFORE AI:  All bets are expensive → only bet on a few things
AFTER AI:   Implementation bets are free → bet on everything

This means:
  - Generate 3 implementations, benchmark all 3
  - Generate 5 test strategies, compare coverage
  - Prototype 3 UX approaches, user-test all 3

The "bet" at the lower levels becomes "try all options."
Only the upper levels (outcome, architecture) still require selective betting.
```

---

## Real Options: When to Commit

```
OPTION VALUE = value of being able to choose later

Commit when:
├── The cost of waiting exceeds the value of more information
├── A deadline forces a decision (external constraint)
├── The test results are conclusive
└── Keeping the option open creates complexity that slows everything

Keep options open when:
├── The test hasn't run yet
├── Results are inconclusive
├── Reversing the decision is expensive
├── More information is arriving naturally (user data, market signals)
└── AI can maintain multiple options cheaply (feature flags, A/B tests)
```

---

## Connecting to Context Windows

Bets affect context window strategy:

```
UNVALIDATED BET:
  Load spec as PROVISIONAL — mark assumptions clearly
  AI generates with explicit uncertainty markers
  Tests include assumption validation tests

VALIDATED BET:
  Load spec as COMMITTED — assumptions are now facts
  AI generates with full confidence
  Tests focus on correctness, not assumption validation

INVALIDATED BET:
  Remove from context entirely — don't waste tokens on dead bets
  Archive the spec with lessons learned
  Update outcome/decomposition if affected
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Identify assumptions | Reviews AI's list | Extracts assumptions from specs and decisions |
| Rank risk | Validates scoring | Proposes risk scores from historical patterns |
| Design tests | Validates approach | Proposes cheapest test for each assumption |
| Execute tests | Monitors high-stakes tests | Executes implementation-level tests fully |
| Interpret results | Judges complex results | Reports clear results, flags ambiguous ones |
| Commit/delay decisions | Makes the call | Models option value and commitment cost |
| Generate alternatives | Selects from options | Generates multiple options cheaply |
