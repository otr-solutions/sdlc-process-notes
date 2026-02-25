# Mechanism 4: ADAPT

> **"Respond to environmental change — evolve the system to maintain fitness."**

## Purpose

When FIT detects declining fitness, ADAPT determines the appropriate response. Not every fitness change requires the same response — some need spec tweaks, others need complete outcome reassessment.

---

## Adaptation Levels

### Level 1: Parameter Adjustment (Smallest)
```
TRIGGER:    Fitness slightly off; environment changed marginally
RESPONSE:   Adjust values within existing spec
EXAMPLE:    "Reminder timing shifted from 24hr to 18hr based on open rate data"
COST:       Spec edit + regeneration (minutes)
FREQUENCY:  Common (weekly)
```

### Level 2: Spec Evolution
```
TRIGGER:    Fitness declining; current rules don't fit behavior
RESPONSE:   Add/modify/remove rules in existing specs
EXAMPLE:    "Added weekend-aware scheduling — users don't open reminders on weekends"
COST:       Spec update + regeneration + new tests (hours)
FREQUENCY:  Regular (per sprint)
```

### Level 3: Boundary Shift
```
TRIGGER:    Unit outgrowing context budget or boundaries no longer align
RESPONSE:   Decompose, merge, or redraw unit boundaries
EXAMPLE:    "reminder-scheduling split into scheduling-logic and channel-selection"
COST:       New decomposition + new specs + regeneration (days)
FREQUENCY:  Occasional (quarterly)
```

### Level 4: Outcome Pivot
```
TRIGGER:    Outcome no longer relevant or achievable
RESPONSE:   Redefine the outcome; cascade changes through all layers
EXAMPLE:    "Shifted from 'reduce no-shows' to 'maximize rescheduling' after
             discovering users want flexibility, not reminders"
COST:       New outcome + new decomposition + new specs (weeks)
FREQUENCY:  Rare (semi-annually)
```

### Level 5: Paradigm Shift
```
TRIGGER:    Fundamental technology or market disruption
RESPONSE:   Rethink the entire approach
EXAMPLE:    "AI can now directly negotiate appointment times with users —
             the entire reminder concept is obsolete"
COST:       New product vision (months)
FREQUENCY:  Very rare (years)
```

---

## Matching Response to Signal

```
FITNESS DROP         SIGNAL STRENGTH    APPROPRIATE LEVEL
──────────────       ───────────────    ─────────────────
Small, gradual       Weak               Level 1 (parameter)
Moderate, consistent Medium             Level 2 (spec evolution)
Large, structural    Strong             Level 3 (boundary shift)
Fundamental          Very strong         Level 4 (outcome pivot)
Industry-wide        Overwhelming       Level 5 (paradigm shift)
```

### The Over-Adaptation Anti-Pattern
```
SYMPTOM:  Every small signal triggers a Level 3-4 response
DANGER:   Constant churn; nothing stabilizes; team exhausted
FIX:      Match response level to signal strength
          Most signals are Level 1-2; Level 3+ is rare
```

### The Under-Adaptation Anti-Pattern
```
SYMPTOM:  Fitness declining for weeks but only Level 1 adjustments
DANGER:   Boiling frog — gradual decline isn't dramatic enough to trigger change
FIX:      Automated threshold triggers
          "If fitness below target for 3 consecutive sprints → escalate to Level 3 review"
```

---

## Adaptation and Context Windows

Each adaptation level has different context window implications:

```
LEVEL 1: Same context, tweaked values       → No context change
LEVEL 2: Updated spec, same boundaries      → Regenerate from updated spec
LEVEL 3: New boundaries, new specs          → New context manifests needed
LEVEL 4: New outcome, cascade through all   → Rebuild all context from scratch
LEVEL 5: New everything                     → Start from zero
```

The beauty of the AI-era model: **Levels 1-3 are cheap.** Regeneration from updated specs costs minutes, not months. This makes the system much more adaptable than pre-AI architectures where every change required manual rework.

---

## The Red Queen Principle

Van Valen's Red Queen: "You must keep running just to stay in the same place."

In SDLC terms: **the environment evolves even when you don't.** Competitors improve, user expectations rise, technology advances. Standing still means falling behind.

```
MINIMUM VIABLE ADAPTATION:
├── Weekly: Review fitness dashboard (5 min)
├── Sprint: Level 1-2 adjustments as needed
├── Quarterly: Evaluate whether Level 3 is needed
├── Annually: Question whether the outcome is still right (Level 4 check)
└── Continuously: AI monitors for punctuated equilibrium triggers
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Detect adaptation need | Reviews fitness trends | Monitors and alerts on thresholds |
| Classify adaptation level | Judges signal strength | Proposes level from signal patterns |
| Execute Level 1-2 | Approves spec changes | Updates specs, regenerates code |
| Execute Level 3 | Leads boundary redesign | Proposes new decompositions |
| Execute Level 4-5 | Leads strategic pivot | Provides data for decision |
| Prevent over/under-adaptation | Monitors response proportionality | Tracks adaptation level vs. signal strength |
