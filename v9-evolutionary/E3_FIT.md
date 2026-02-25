# Mechanism 3: FIT

> **"Measure fitness against the environment — is alignment holding?"**

## Purpose

Fitness is the measure of how well the system serves its intended outcome in the **current environment**. High fitness today doesn't guarantee fitness tomorrow. FIT is continuous measurement that detects when adaptation is needed.

---

## Fitness Functions

Automated, measurable checks that evaluate system health across dimensions:

### Outcome Fitness
```
FUNCTION:    Does the system drive the intended outcome?
MEASURE:     Primary outcome metric (e.g., missed appointment rate)
FREQUENCY:   Continuous (real-time dashboards)
THRESHOLD:   Below target + declining trend → trigger ADAPT
OWNER:       AI monitors, human judges significance
```

### Structural Fitness
```
FUNCTION:    Does the architecture support AI-era constraints?
MEASURES:    
  - % of units within context window budget
  - Average coupling between units
  - Dependency depth (how many hops between units)
FREQUENCY:   Per commit (automated)
THRESHOLD:   Any unit >80% context budget → flag for decomposition
OWNER:       AI monitors and flags
```

### Alignment Fitness
```
FUNCTION:    Is all work connected to an active outcome?
MEASURES:
  - Traceability coverage (code → outcome %)
  - Contract drift (days since last review)
  - Spec clarity scores (AI question count)
FREQUENCY:   Weekly
THRESHOLD:   Coverage <90% or drift >30 days → flag
OWNER:       AI monitors, human reviews flags
```

### Adaptability Fitness
```
FUNCTION:    Can the system change quickly when needed?
MEASURES:
  - Lead time (spec change → deployed code)
  - Regeneration rate (% code generated vs. hand-written)
  - Test coverage by generation type
FREQUENCY:   Per sprint
THRESHOLD:   Lead time increasing → investigate bottleneck
OWNER:       AI tracks, human investigates
```

---

## Fitness Landscape Awareness

The fitness landscape isn't static — it shifts as the environment changes:

```
ENVIRONMENT SHIFTS THAT CHANGE FITNESS:
├── Market change: Competitor ships a similar feature
│   → Your feature's value (fitness) drops even though nothing changed
├── User change: User population shifts demographics
│   → Assumptions about behavior may no longer hold
├── Technology change: New AI capabilities available
│   → What was impossible is now possible; old approaches suboptimal
├── Regulatory change: New compliance requirements
│   → Previously fit architecture may now be non-compliant
└── Team change: Key people leave or join
│   → Conway alignment shifts; ownership may not match architecture
```

FIT detects these shifts through the fitness functions. ADAPT responds to them.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define fitness functions | Decides what matters | Proposes functions from outcome definition |
| Measure fitness | Reviews dashboards | Monitors continuously, alerts on thresholds |
| Detect environment shifts | Notices market/org changes | Detects metric anomalies and trend breaks |
| Interpret fitness changes | Judges whether adaptation needed | Correlates across fitness dimensions |
| Trigger ADAPT | Decides when to act | Recommends timing based on trends |
