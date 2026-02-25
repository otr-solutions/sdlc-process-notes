# Mechanism 2: SELECT

> **"Choose what survives. This is where human judgment is irreplaceable."**

## Purpose

Variation without selection is chaos. SELECT is the mechanism that applies **judgment** to choose which options advance and which are discarded. In biological evolution, the environment selects. In software, **humans select based on alignment to outcomes.**

---

## Selection Criteria

### For Outcome Selection
```
CRITERIA:
├── Does it connect to business strategy?
├── Is it measurable?
├── Is it achievable with current resources?
├── Does it matter more than the alternatives?
└── Would stakeholders recognize it as valuable?

SELECTOR: Human (values, strategy, authority)
```

### For Architecture/Decomposition Selection
```
CRITERIA:
├── Do units fit in context windows?
├── Do boundaries align to team structure?
├── Will boundaries hold as requirements evolve?
├── Is the dependency graph manageable?
└── Can each unit be built independently?

SELECTOR: Human (strategic bet) with AI analysis (coupling, budgets)
```

### For Implementation Selection
```
CRITERIA:
├── Does it pass all automated gates?
├── Performance within acceptable range?
├── Maintainability score acceptable?
├── Dependencies within policy?
└── Security scan clean?

SELECTOR: AI (automated, measurable criteria)
          Human only for tie-breaking or edge cases
```

---

## Selection Pressure

In biology, selection pressure determines what traits survive. In SDLC:

```
STRONG SELECTION PRESSURE (kill bad options fast):
├── Automated gates: compile, test, security (immediate)
├── Spec compliance check (minutes)
├── Context budget check (immediate)
└── These eliminate obviously unfit options before human review

MODERATE SELECTION PRESSURE (evaluate thoughtfully):
├── Alignment review: does this serve the outcome?
├── Integration check: does this compose with adjacent units?
└── These require human judgment but on AI-pre-filtered options

WEAK SELECTION PRESSURE (observe over time):
├── Outcome metrics: is the metric moving?
├── User behavior: are users doing what we expected?
└── These only become visible after deployment + observation period
```

### The AI Advantage in Selection

AI applies strong selection pressure instantly and cheaply. Without AI, developers would manually test, review, and benchmark each option. With AI:

```
BEFORE AI:  Generate 1 option, spend days evaluating it
AFTER AI:   Generate 5 options, AI eliminates 3 instantly via gates,
            human evaluates 2 finalists
```

---

## Selection Anti-Patterns

### 1. HiPPO (Highest Paid Person's Opinion)
```
SYMPTOM:  Selection based on authority, not criteria
FIX:      Pre-define criteria before seeing options
```

### 2. Analysis Paralysis
```
SYMPTOM:  Can't choose between similar options
FIX:      If options are similar, they're interchangeable. Pick one.
          The cost of deliberating exceeds the cost of being slightly wrong.
```

### 3. Sunk Cost Selection
```
SYMPTOM:  Keeping a bad option because we've invested in it
FIX:      AI makes regeneration cheap. There's no sunk cost when
          you can regenerate from spec in minutes.
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define selection criteria | Authors before variation starts | Suggests criteria from patterns |
| Apply strong selection (gates) | Nothing (automated) | Eliminates unfit options instantly |
| Apply moderate selection (alignment) | Judges finalists | Pre-filters and ranks options |
| Apply weak selection (metrics) | Interprets over time | Collects and trends data |
| Prevent anti-patterns | Self-monitors for bias | Flags when criteria aren't being applied |
