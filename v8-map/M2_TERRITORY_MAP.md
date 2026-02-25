# Territory Map

> **"What exists right now — the honest, unvarnished reality?"**

## Purpose

The territory map is the **ground truth** — not what we planned, not what we wish, but what IS. It covers the codebase, infrastructure, teams, constraints, and risks as they exist today.

## What It Contains

```
CODEBASE:
├── Component inventory (what exists, age, health)
├── Sacred cows (components nobody is allowed to change)
├── Technical debt (known issues, workarounds, landmines)
├── Test coverage by component
└── Context window budget by unit (token counts)

INFRASTRUCTURE:
├── Current architecture (actual, not diagram from 2 years ago)
├── Dependencies and their status (contracts, versions, health)
├── Environment differences (dev vs. staging vs. prod)
└── Capacity and scaling limits

TEAMS:
├── Who owns what (actual ownership, not org chart)
├── Expertise distribution (who knows what)
├── Capacity and availability (who's actually available)
├── Knowledge risks (bus factor per component)

CONSTRAINTS:
├── Regulatory requirements (HIPAA, GDPR, SOC2)
├── Contract obligations (vendor SLAs, customer promises)
├── Timeline pressures (deadlines, dependencies)
└── Political realities (what's actually negotiable)
```

## Why AI Must Maintain This

Humans are bad at keeping territory maps current. They document what they build, then the territory drifts:

```
AI can:
├── Scan the codebase for component inventory
├── Analyze git history for ownership and bus factor
├── Track dependency versions and health
├── Monitor infrastructure state continuously
├── Flag when territory map diverges from reality
```

Humans add what AI can't see: political realities, strategic constraints, and tacit knowledge about fragility.

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Maintain technical inventory | Validates | Scans and updates continuously |
| Document sacred cows | Authors | Cannot determine these independently |
| Track team capacity | Provides updates | Aggregates from project tools |
| Identify constraints | Authors political/strategic | Detects technical constraints |
| Flag staleness | Reviews periodically | Alerts when map diverges from reality |
