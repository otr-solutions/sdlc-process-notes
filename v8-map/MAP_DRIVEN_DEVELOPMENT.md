# V8: Map-Driven Development

> **Core Question: "Do all participants — human and AI — share the same map of the territory?"**

## The Philosophical Difference

- V1-V7 focus on process, cognition, and information
- V8 focuses on **shared situational awareness** — you can't align if people are looking at different maps

## Why This Dimension Matters

"The map is not the territory" (Korzybski). But in software, **the map IS the primary artifact**. Code is a map of behavior. Specs are maps of intent. Architecture diagrams are maps of structure. Misalignment happens when participants use **different maps** of the same territory — or when the territory has changed but the maps haven't.

---

## Foundation Models

| Model | Era | Core Insight | V8 Application |
|---|---|---|---|
| Wardley Mapping (Wardley, 2005) | 00s | Map components by value chain position + evolution stage | Know what's commodity vs. novel; invest attention accordingly |
| Value Stream Mapping (Toyota/Lean) | 80s | Visualize flow and identify waste | See where alignment value is created and lost |
| Context Mapping (DDD, Evans, 2003) | 00s | Relationships between bounded contexts | Map how domains relate and where translation is needed |
| Event Storming (Brandolini, 2013) | 10s | Discover domain events collaboratively | Build shared maps through collaborative discovery |
| Cynefin Mapping (Snowden) | 00s | Map decisions to domains | Know which map type is appropriate for each situation |

---

## The V8 Model: Four Map Types

Each map reveals something the others don't. Together, they provide complete situational awareness.

```
┌─────────────────────────────────────────────────────────┐
│                  MAP-DRIVEN DEVELOPMENT                  │
│                                                          │
│  ┌──────────────┐        ┌──────────────┐                │
│  │  VALUE MAP   │        │ TERRITORY MAP│                │
│  │              │        │              │                │
│  │ "Where is    │        │ "What exists │                │
│  │  value       │        │  today?"     │                │
│  │  created?"   │        │              │                │
│  └──────────────┘        └──────────────┘                │
│                                                          │
│  ┌──────────────┐        ┌──────────────┐                │
│  │ CONTEXT MAP  │        │ JOURNEY MAP  │                │
│  │              │        │              │                │
│  │ "How do      │        │ "How does    │                │
│  │  domains     │        │  work/value  │                │
│  │  relate?"    │        │  flow?"      │                │
│  └──────────────┘        └──────────────┘                │
└─────────────────────────────────────────────────────────┘
```

---

## Map 1: Value Map (Wardley-Inspired)

### What It Shows
Where each component sits on two axes: **value chain position** (visibility to user) and **evolution stage** (genesis → custom → product → commodity).

```
         VISIBLE TO USER
              ▲
              │
  User Need:  │  ○ Appointment Booking
  "Don't miss │  │
  appointments"│  ○ Reminder System ←── Building this
              │  │
              │  ○ Notification Delivery
              │  │
              │  ○ SMS Provider (commodity)
              │
  INVISIBLE   ○ Infrastructure
              │
              └──────────────────────────────────►
              GENESIS    CUSTOM    PRODUCT    COMMODITY
              (novel)                         (standard)
```

### Why It Matters for AI SDLC
- **Commodity components** (right side) → T4/T5 trust levels, AI-generated, don't spend alignment attention
- **Custom/novel components** (left side) → T1/T2 trust levels, human alignment-critical
- **Evolution direction** → components move right over time; today's custom is tomorrow's commodity

### Context Window Implication
Don't load commodity component details into context. They're solved problems. Load the novel/custom components that need alignment attention.

---

## Map 2: Territory Map

### What It Shows
The current state of the codebase, infrastructure, and organizational reality. Not what we want — what IS.

```
TERRITORY MAP:
├── Codebase
│   ├── Legacy auth service (don't touch — sacred cow)
│   ├── Booking API (stable, well-tested, 3 years old)
│   ├── Notification service (new, under-tested, 2 months)
│   └── Scheduling engine (doesn't exist yet)
│
├── Infrastructure
│   ├── AWS us-east-1 (primary)
│   ├── PostgreSQL 14 (main database)
│   └── SMS via Twilio (contract expires Q3)
│
├── Teams
│   ├── Team A: 4 devs, owns booking (experienced)
│   ├── Team B: 3 devs, owns notifications (new team)
│   └── Team C: 2 devs, shared/platform (understaffed)
│
└── Constraints
    ├── HIPAA compliance required
    ├── Twilio contract under renegotiation
    └── Team C lead leaving in March
```

### Why It Matters
Alignment decisions must account for territory reality. A perfect decomposition that ignores "Team C lead leaving in March" will fail in practice.

---

## Map 3: Context Map (DDD-Inspired)

### What It Shows
How bounded contexts (domains) relate to each other. Where translation is needed. Where alignment risk lives at boundaries.

```
┌────────────┐     SHARED KERNEL      ┌─────────────┐
│  SCHEDULING │◄────(appointment)────►│  BOOKING     │
│  CONTEXT    │                        │  CONTEXT     │
└──────┬──────┘                        └──────────────┘
       │
       │ CUSTOMER-SUPPLIER
       │ (scheduling publishes events;
       │  delivery consumes)
       │
┌──────▼──────┐     ANTI-CORRUPTION    ┌─────────────┐
│  DELIVERY   │◄────LAYER────────────►│  TWILIO      │
│  CONTEXT    │     (translate our     │  (external)  │
└─────────────┘      model to Twilio's)└─────────────┘
```

### Relationship Types That Matter

```
SHARED KERNEL:      Two contexts share a model. Changes require both to agree.
                    → High alignment cost. Minimize shared kernels.

CUSTOMER-SUPPLIER:  Upstream publishes, downstream consumes.
                    → Consumer-driven contracts (V6 Interfaces).

ANTI-CORRUPTION:    Translation layer between our model and external systems.
                    → Isolates external noise from internal signal (V7).

CONFORMIST:         We adopt someone else's model as-is.
                    → Low cost but high coupling. Use for commodities.
```

---

## Map 4: Journey Map

### What It Shows
How value flows through the system from user need to delivered outcome. Where bottlenecks and waste exist.

```
USER JOURNEY: "I might miss my appointment"

  ┌─────┐   ┌─────────┐   ┌────────┐   ┌──────────┐   ┌────────┐
  │ Book │──→│ Reminder │──→│ Open   │──→│ Act      │──→│ Attend │
  │ appt │   │ sent     │   │ reminder│  │(confirm/ │   │ appt   │
  │      │   │          │   │        │   │ reschedule)│  │        │
  └──────┘   └──────────┘   └────────┘   └──────────┘   └────────┘
  
  ALIGNMENT QUESTIONS AT EACH STEP:
  Book:     "Is the booking experience setting up success?"
  Sent:     "Is the reminder reaching the user through the right channel?"
  Open:     "Is the reminder compelling enough to open?"
  Act:      "Is the action (confirm/reschedule) easy enough?"
  Attend:   "Did the outcome actually change?"
```

---

## Maps and Context Windows

Each map type has a different context window strategy:

```
VALUE MAP:        Load into strategic planning sessions (INTENT station)
                  ~2K tokens compressed

TERRITORY MAP:    Load relevant sections per unit
                  Full map too large; slice by domain
                  ~3-5K tokens per slice

CONTEXT MAP:      Load adjacent context relationships only
                  ~1-2K tokens (interface definitions)

JOURNEY MAP:      Load for integration review (SHAPE station)
                  ~2-3K tokens (steps + alignment questions)
```

---

## Comparison

| Map | Shows | Updates | Primary User |
|---|---|---|---|
| Value | Where to invest attention | Quarterly | Leadership, architects |
| Territory | What exists now | Continuously (AI-maintained) | Everyone |
| Context | How domains relate | Per decomposition change | Architects, team leads |
| Journey | How value flows | Per outcome change | Product, design, engineering |

→ See map documents: [Value](VALUE_MAP.md) · [Territory](TERRITORY_MAP.md) · [Context](CONTEXT_MAP.md) · [Journey](JOURNEY_MAP.md)
