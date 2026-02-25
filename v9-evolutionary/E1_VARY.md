# Mechanism 1: VARY

> **"Generate diverse options. AI makes variation nearly free."**

## Purpose

Evolution requires variation — multiple options to select from. Before AI, generating options was expensive (each prototype took weeks). Now, AI can generate multiple implementations, spec drafts, or architecture proposals in minutes.

VARY is the mechanism that exploits this cost collapse.

---

## Where Variation Applies

### Outcome Variations
```
"Here are 5 possible outcomes we could pursue based on customer data,
 each with projected impact and confidence levels."

AI generates: multiple outcome proposals from the same data
Human selects: which outcome to pursue (values judgment)
```

### Architecture Variations
```
"Here are 3 decompositions of this system, each optimizing for
 different change scenarios."

AI generates: decomposition options with trade-off analysis
Human selects: which boundaries to commit to (strategic bet)
```

### Implementation Variations
```
"Here are 3 implementations of this spec — one optimizing for
 readability, one for performance, one for minimal dependencies."

AI generates: multiple implementations from the same spec
Automated selection: benchmark, compare, choose (no human needed)
```

### Spec Variations
```
"Here are 2 ways to handle the quiet hours edge case — one strict
 (never send during quiet hours) and one flexible (send if urgent)."

AI generates: alternative rule sets
Human selects: which rule matches intent
```

---

## Variation Budget

Not everything needs variation. Use variation where **uncertainty is highest**:

```
HIGH UNCERTAINTY → Generate 3-5 options:
├── Outcome definition (what to build)
├── Decomposition boundaries (how to structure)
├── Complex business rules (how edge cases work)
└── UX approaches (how users interact)

LOW UNCERTAINTY → Generate 1 option:
├── CRUD implementations (well-understood patterns)
├── Commodity integrations (standard approaches)
├── Infrastructure configuration (best practices exist)
└── Test generation (derived from spec mechanically)
```

---

## Context Window Strategy for Variation

```
VARIATION GENERATION:
  Load: spec + "generate N alternatives with trade-off analysis"
  Budget: Same context per variation (parallel, not sequential)
  
VARIATION COMPARISON:
  Load: all N variations + selection criteria
  Budget: May need larger context to compare
  Strategy: AI generates comparison matrix, human reviews matrix
            (not individual variations)
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Decide where to generate variations | Identifies high-uncertainty areas | Proposes based on decision confidence |
| Generate variations | Nothing (offloaded) | Generates N options with trade-offs |
| Compare variations | Reviews comparison matrix | Generates matrix, benchmarks implementations |
| Select from variations | Chooses (values/strategy judgment) | Recommends based on criteria |
