# Phase 4: LEARN

> **"Were we right — and what do we know now that we didn't before?"**

## Purpose

Close the OODA loop. Measure the outcome of bets, update mental models, and feed learning back into SENSE. Without LEARN, you're making the same bets over and over without improving.

---

## What Learning Looks Like

### Validated Learning (Lean Startup)

Not "we learned a lot" — but **"we now know X with Y confidence because of Z evidence."**

```
BET:      SMS reminders will reduce no-shows
TEST:     500 users, 2 weeks, SMS reminders 24hr before
RESULT:   Open rate: 92%. Action rate: 41%. No-show rate: 14% (from 23%)
LEARNING: SMS reminders reduce no-shows by ~40% relative.
          VALIDATED — but not enough alone to reach <10% target.
          Need additional intervention (reschedule flow?).
CONFIDENCE: HIGH (large sample, clear causation, controlled test)

NEXT BET: "Adding one-tap reschedule will get no-shows below 10%"
```

### Learning Types

```
CONFIRMATION:   "Our assumption was right. Commit and move on."
                → Update spec from PROVISIONAL to COMMITTED
                → Reduce future testing on this dimension

SURPRISE:       "Our assumption was wrong in an unexpected way."
                → Revise the model fundamentally
                → This often changes decomposition or outcome

PARTIAL:        "Our assumption was partly right."
                → Refine the spec with nuance
                → Design a more targeted follow-up bet

INCONCLUSIVE:   "We can't tell from this test."
                → Design a better test
                → The test was wrong, not necessarily the assumption
```

---

## The Learning Loop

```
LEARN feeds back into each earlier phase:

LEARN → SENSE:   "Now that we know X, watch for Y"
                  Update monitoring criteria

LEARN → FRAME:   "This problem is more Complex than we thought"
                  Reclassify the domain

LEARN → BET:     "This assumption is validated, remove from risk stack"
                  Update bet portfolio

LEARN → LEARN:   "Our learning process itself has gaps"
                  Improve how we test and measure (meta-learning)
```

---

## Learning and Context Windows

Learning updates what gets loaded into AI context:

```
BEFORE LEARNING:
  Context loads: "ASSUMPTION: Users read SMS within 1 hour"

AFTER LEARNING:
  Context loads: "VALIDATED: Users read SMS within 5 minutes (92% open rate,
                  N=500, 2-week test, Feb 2026)"

The shift from assumption to validated fact changes how AI reasons:
  - Less hedging in generated code
  - Fewer alternative paths to maintain
  - Tighter specs with fewer conditionals
  - Smaller context footprint (certainty is more compact than uncertainty)
```

### Certainty Compresses Context

Uncertain specs are large (many edge cases, alternatives, conditionals). Validated specs are small (clear rules, known behaviors). **Learning literally shrinks context window requirements.**

```
BEFORE:  "If users prefer SMS (assumed), send SMS. But also support
          push and email as fallbacks in case SMS doesn't work.
          Consider quiet hours for all channels..."
          → ~500 tokens of hedged specification

AFTER:   "Send SMS (validated preferred channel, 92% open rate).
          Fallback to push only if SMS delivery fails."
          → ~100 tokens of confident specification
```

---

## Learning Artifacts

### The Learning Log

```
DATE:       2026-02-20
BET:        SMS reminders reduce no-shows
RESULT:     Partially validated — reduces from 23% to 14%
LEARNING:   SMS alone not sufficient for <10% target
IMPACT:     
  - Outcome: Target achievable with additional intervention
  - Decomposition: reschedule-flow now critical path
  - Spec: reminder-scheduling spec COMMITTED
  - Risk stack: Remove "SMS works" risk; Add "reschedule impact" risk
NEXT:       Bet on reschedule-flow impact
```

### Pattern Library

Over time, learnings accumulate into reusable patterns:

```
PATTERN: "Notification channels follow power law"
EVIDENCE: 3 projects showed SMS dominates, push is distant second
APPLIES WHEN: User notification channel selection
IMPLICATION: Design for one primary channel, not equal multi-channel
CONFIDENCE: Medium (3 data points, same industry)
```

The pattern library loads into AI context for future projects — making the organization smarter over time.

---

## Learning Anti-Patterns

### 1. Confirmation Bias
```
SYMPTOM:  Only measuring things that confirm the bet
FIX:      Pre-define decision criteria BEFORE running the test
          Include criteria for invalidation, not just validation
```

### 2. Vanity Metrics
```
SYMPTOM:  "We sent 10,000 reminders!" (activity, not outcome)
FIX:      Only measure the outcome metric from Layer 1
          Activity metrics are noise unless they correlate to outcomes
```

### 3. Learning Without Updating
```
SYMPTOM:  "We learned X" but specs/context unchanged
FIX:      Every learning MUST update at least one artifact
          If nothing changed, you didn't really learn
```

### 4. Over-Learning
```
SYMPTOM:  Running more tests when the answer is clear
FIX:      Pre-defined decision criteria prevent endless experimentation
          "If >80% open rate, commit" — don't wait for 95% confidence
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define decision criteria | Sets before test runs | Suggests based on statistical power |
| Measure results | Validates methodology | Collects and analyzes data |
| Classify learning type | Judges (confirmation/surprise/partial) | Proposes classification |
| Update artifacts | Approves changes | Implements updates across specs/context |
| Build pattern library | Validates patterns | Identifies recurring patterns |
| Detect anti-patterns | Self-monitors | Flags confirmation bias, vanity metrics |
| Feed back to SENSE | Defines new monitoring criteria | Updates signal filters |
