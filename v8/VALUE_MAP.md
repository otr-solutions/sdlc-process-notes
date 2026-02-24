# Value Map

> **"Where is value created — and where should attention be invested?"**

Inspired by Wardley Mapping. Plots components on two axes: **visibility to user** and **evolution stage**.

## How to Build

```
1. Start from user need (top of the value chain)
2. List every component needed to serve that need
3. Place each on the evolution axis:
   - Genesis: Never been done before (experiment)
   - Custom: Done before but tailored to us (build)
   - Product: Off-the-shelf with configuration (buy/configure)
   - Commodity: Utility, no differentiation (use standard)
4. Draw dependencies (what needs what)
```

## AI SDLC Implications

```
EVOLUTION STAGE    AI TRUST LEVEL    ATTENTION LEVEL    CONTEXT BUDGET
───────────────    ──────────────    ───────────────    ──────────────
Genesis            T1-T2             Maximum            Large (full spec + exploration)
Custom             T2-T3             High               Standard (spec + generation)
Product            T3-T4             Low                Small (config + conventions)
Commodity          T4-T5             None               Minimal (automated)
```

Components evolve rightward over time. **Today's genesis is next year's commodity.** The value map tells you where the alignment boundary will shift — and when to start offloading.

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Position components on axes | Judges evolution stage | Suggests from market analysis |
| Identify dependencies | Validates | Derives from code/architecture |
| Predict evolution direction | Strategic judgment | Trends from industry data |
| Update the map | Reviews quarterly | Flags components that have evolved |
