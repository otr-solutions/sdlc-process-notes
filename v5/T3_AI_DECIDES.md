# T3: AI Decides, Human Reviews

> **AI acts first. Human reviews after the fact and can override.**

## When to Use

```
├── The decision is REVERSIBLE (regenerate from spec if wrong)
├── The domain is COMPLICATED but PATTERNED (good practices exist)
├── AI has a PROVEN TRACK RECORD at this activity type
├── Speed matters — waiting for human approval creates bottlenecks
└── Automated gates catch most mechanical errors
```

## Activities at This Level

```
├── Implementation code generation (from validated spec)
├── Test generation (from validated spec)
├── Edge case enumeration (human validates business relevance)
├── API documentation generation
├── Change impact analysis
├── Traceability updates
└── Spec-to-code compliance verification
```

## How It Works

```
1. AI receives validated spec (from T2-approved contract)
2. AI generates the artifact
3. Automated gates run (compile, test, coverage, security)
4. IF gates pass → artifact is provisionally live
5. Human reviews asynchronously (within review window)
6. Human can: approve, request changes, or override
```

### The Key Shift from T2

At T2, nothing happens until human approves.
At T3, **work proceeds while human reviews.** The human has a window to override, but the default is that AI's decision stands if the human doesn't intervene.

```
T2 flow: AI drafts → Human reviews → THEN it ships
T3 flow: AI decides → Ships provisionally → Human reviews async
```

This is the difference between **blocking review** and **non-blocking review**.

## Review Strategy

At T3, human review is lighter — sampling, not comprehensive:

```
Review depth: 2-5 minutes per artifact (spot-check)
Review frequency: Every artifact initially, then sampling as trust builds
Review focus: 
  - Surprising patterns (does something look off?)
  - Alignment spot-checks (pick one rule, verify it's implemented)
  - Gate failure patterns (why did any gates fail?)

NOT reviewing:
  - Every line of code
  - Test implementation details
  - Documentation wording
  - Formatting or style
```

## Context Window Strategy

```
AI context load: Standard (spec + shared context)
Human review load: MINIMAL
  - AI-generated summary of what changed
  - Diff view highlighting key decisions
  - Gate results dashboard
  - Only load full artifact if summary raises concerns
```

## Graduation Criteria (T3 → T4)

```
├── Override rate <5% for 30+ artifacts
├── No critical issues missed in last 50 reviews
├── Automated gates catch 100% of mechanical errors
├── Human review adds value <10% of the time
└── Activity is COMMODITY (not differentiated, not strategic)
```

## Trust Metrics at T3

```
Override Rate:      % of AI decisions human overrides
                    <5% sustained → candidate for T4

Review Yield:       % of reviews where human catches something meaningful
                    <10% → human review may be extraneous load

Time-to-Review:     How long artifacts wait for human review
                    Long waits → either promote to T4 or add reviewers

Gate Reliability:   Do automated gates catch what they should?
                    Track false negatives (things gates missed that humans caught)
```

## Anti-Pattern: Non-Blocking but Never Reviewing

```
SYMPTOM:  Reviews pile up, nobody looks at them
CAUSE:    T3 review is non-blocking, so there's no pressure
DANGER:   Problems accumulate undetected → automation bias
FIX:      
  - Set a review SLA (e.g., within 24 hours)
  - AI summarizes outstanding reviews daily
  - Reduce review scope to make it achievable
  - Promote to T4 with guardrails if reviews aren't adding value
```
