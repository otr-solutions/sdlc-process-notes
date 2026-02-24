# Mode 2: CHUNK

> **"Package everything for bounded cognition — human AND machine."**

## Purpose

Transform all artifacts into pieces that fit within cognitive limits. This applies equally to **human working memory** (~7±2 chunks) and **AI context windows** (~128K tokens). The same chunking strategy serves both.

---

## Miller's Law Applied to Software

Miller (1956) showed that humans can hold ~7±2 items in working memory. But the items can be **chunks** — grouped, meaningful units. An expert chess player doesn't see 32 pieces; they see 5-6 patterns.

The implication for SDLC:

```
BAD:  Present a developer with 47 unrelated requirements
      → Overwhelms working memory
      → Details are lost, connections missed

GOOD: Present 5 themed groups of requirements, each with clear boundaries
      → Fits in working memory as 5 chunks
      → Relationships between groups are visible
```

This is why V1's decomposition and V2's shaping matter — they create chunks. V3 makes the cognitive reasoning explicit.

---

## Chunking Rules

### Rule 1: The 7±2 Rule for Human Review

When a human reviews anything, they should encounter **no more than 7 primary elements**:

```
A contract should have:       ≤7 rules (group edge cases separately)
A decomposition should have:  ≤7 units (if more, create sub-groups)
A review should cover:        ≤7 items (batch reviews if needed)
A dashboard should show:      ≤7 metrics (hide secondary metrics)
A decision should consider:   ≤7 options (AI pre-filters)
```

### Rule 2: The Context Window Rule for AI

Every AI task should load **≤60% of context window**, leaving 40% for reasoning:

```
Shared context (outcome, glossary):   ~10K tokens  (fixed overhead)
Task-specific context (spec, code):   ~30K tokens  (variable)
────────────────────────────────────────────────────
Total loaded:                         ~40K tokens  (60% of ~67K usable)
Reasoning headroom:                   ~27K tokens  (40% for generation)
```

### Rule 3: Chunk Boundaries = Information Hiding Boundaries

A chunk should be **self-contained**: understanding the chunk doesn't require loading another chunk into memory. This is Parnas's information hiding, restated cognitively:

```
GOOD CHUNK:  "reminder-scheduling handles when reminders fire"
             → Understandable without knowing how delivery works

BAD CHUNK:   "reminder-scheduling handles when reminders fire,
              but you need to understand delivery retry logic
              to know why we chose these timings"
             → Requires loading another chunk → overflows memory
```

---

## Chunking Strategies by Artifact Type

### Outcome Documents
```
Chunk by: theme/opportunity
Target:   ≤5 opportunity branches per outcome
Each:     1 paragraph + 3-5 metrics

If larger: split into sub-outcomes with explicit relationships
```

### Decomposition Maps
```
Chunk by: domain/capability
Target:   ≤7 units per group
Each:     Unit card (~500 words)

If larger: create a hierarchy
  System → Domains (≤7) → Units per domain (≤7)
  Each level fits in working memory
```

### Contracts/Specs
```
Chunk by: rule group
Target:   ≤7 primary rules
Each:     1-2 sentences with clear pre/post conditions

If larger: split into sub-contracts
  Main contract → references sub-contracts for complex rule groups
  Each sub-contract fits in a context window independently
```

### Code Reviews (Human)
```
Chunk by: alignment concern
Target:   ≤7 review items per session
Each:     One question: "Does this align with [specific outcome/rule]?"

AI pre-screens for correctness; human reviews only alignment items
```

### Metrics Dashboards
```
Chunk by: decision type
Target:   ≤7 visible metrics per view

Primary view:    3-4 outcome metrics
Engineering view: 3-4 DORA metrics
Alignment view:  3-4 alignment health metrics

Each view answers ONE question: "Are we on track for [X]?"
```

---

## Cognitive Chunking Patterns

### Pattern 1: Progressive Disclosure
Show summary first; details on demand. This matches both human cognition (overview first, then drill down) and AI context management (load summary first, load details only for the relevant unit).

```
Level 0: Outcome statement (1 sentence, ~50 tokens)
Level 1: Opportunity tree (5-7 branches, ~500 tokens)
Level 2: Unit cards (per unit, ~1K tokens each)
Level 3: Full specs (per unit, ~5-8K tokens each)
Level 4: Implementation (per unit, ~15-25K tokens each)

Human starts at Level 0, drills to the level needed for their decision.
AI loads Level 0-2 always, Level 3-4 only for the active unit.
```

### Pattern 2: Consistent Internal Structure
Every chunk of the same type should have the **same internal structure**. This reduces cognitive overhead because the reader knows where to look:

```
Every contract:        Purpose → Inputs → Outputs → Rules → Edge Cases → Invariants
Every unit card:       Name → Feature → Hides → Interface → Budget
Every decision record: Context → Decision → Rationale → Consequences
```

The reader's brain learns the pattern once, then processes each instance faster (schema-based chunking).

### Pattern 3: Naming as Chunking
Good names compress information. A well-named unit is a chunk that loads its meaning from the name alone:

```
BAD:   "Service A", "Module 2"           → Name carries no meaning
OK:    "ReminderService"                  → Name hints at domain
GOOD:  "reminder-scheduling"             → Name encodes what it does
BEST:  "Schedule reminder for appointment" → FDD-style, full intent in name
```

---

## Context Window Packing Strategy

When loading an AI context window, pack strategically:

```
ALWAYS LOAD (fixed overhead):
├── Outcome statement        (~200 tokens)
├── Anti-goals               (~200 tokens)
├── Glossary                 (~2K tokens)
├── Relevant schemas         (~2K tokens)
└── Total fixed:             ~4.5K tokens

LOAD FOR CURRENT TASK:
├── Active unit's spec       (~5-8K tokens)
├── Adjacent unit interfaces (~2K tokens, interfaces only)
├── Relevant decision records (~1K tokens)
└── Total task-specific:     ~8-11K tokens

LEAVE FOR GENERATION:
├── Implementation output    (~15-25K tokens)
├── AI reasoning             (~15-20K tokens)
└── Total headroom:          ~30-45K tokens

GRAND TOTAL:                 ~43-60K tokens ✓
```

The key: **load interfaces of adjacent units, never their implementations.** This is information hiding applied to context windows.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define chunk boundaries | Decides based on domain understanding | Proposes based on coupling/size analysis |
| Enforce 7±2 rule | Reviews grouped artifacts | Flags when groups exceed limit |
| Progressive disclosure | Sets what matters at each level | Generates summaries at each level |
| Consistent structure | Defines templates | Enforces templates in generation |
| Context window packing | Validates strategy | Calculates budgets, optimizes packing |
| Naming | Authors meaningful names | Suggests names from spec content |
