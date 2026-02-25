# T2: AI Drafts, Human Decides

> **AI proposes. Human reviews thoroughly and makes the final call.**

## When to Use

```
├── The decision has SIGNIFICANT impact but is somewhat REVERSIBLE
├── The domain is COMPLEX — AI can help but can't fully grasp intent
├── This is the FIRST TIME this type of decision is being made
├── Human needs to BUILD UNDERSTANDING (germane cognitive load)
└── The output shapes DOWNSTREAM decisions significantly
```

## Activities at This Level

```
├── Contract/spec authoring: AI drafts from outcome + decomposition
├── Decomposition boundaries: AI proposes from coupling analysis
├── Architecture decisions: AI proposes options with trade-offs
├── Integration review: AI flags issues, human judges seams
├── Outcome reassessment: AI trends data, human decides pivot/continue
└── Boundary decision records: AI drafts, human authors the "why"
```

## How It Works

```
1. Human provides intent and constraints
2. AI generates a draft (full artifact, not just suggestions)
3. Human reviews the draft thoroughly — this is deep work
4. Human corrects, refines, or rejects
5. Result is human-approved artifact
```

### The Review Matters

At T2, human review is NOT a rubber stamp. It's the primary quality gate:

```
Review depth: 15-30 minutes per artifact
Review focus: Alignment to intent, not correctness
Questions to ask:
  - "Does this capture what I actually mean?"
  - "Would a user recognize this as solving their problem?"
  - "What did AI miss that I know from domain experience?"
  - "What did AI include that I wouldn't have thought of?" (AI adds value here)
```

## Context Window Strategy

```
AI context load:
  - Full outcome definition
  - Full decomposition context
  - All relevant shared artifacts
  - Maximum context for best draft quality

Human review load:
  - AI's draft + AI's reasoning
  - Diff from previous version (if revision)
  - 7±2 key questions to focus on
```

## Graduation Criteria (T2 → T3)

An activity moves to T3 when:
```
├── Human has reviewed 5+ AI drafts of this type
├── Correction rate has dropped below 15%
├── Corrections are minor (wording, edge cases) not structural
├── Human is confident they can catch issues in post-hoc review
└── The cost of a missed issue is recoverable
```

## Trust Metrics at T2

```
Draft Acceptance Rate:  % of drafts accepted with minor changes
                        Tracking >85% → candidate for T3

Correction Severity:    Are corrections structural or cosmetic?
                        Mostly cosmetic → candidate for T3
                        Any structural → stay at T2

Human Value-Add:        What did human catch that AI missed?
                        Track to calibrate AI quality and human review value
```

## Anti-Pattern: Staying at T2 Too Long

```
SYMPTOM:  Human reviews every AI draft even though they never change anything
COST:     Human attention wasted on extraneous review
FIX:      Track acceptance rate. If >90% for 20+ reviews, promote to T3.
```
