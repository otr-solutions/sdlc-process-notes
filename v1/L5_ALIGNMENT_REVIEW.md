# Layer 5: Alignment Review

> **Ownership: AI-Screened, Human-Judged**

## Purpose

Answer the question AI cannot answer: **"Is this what we actually meant?"** This is the Fagan Inspection, reborn — but scoped exclusively to alignment, not correctness. Correctness is handled by automated gates in Layer 4.

---

## What Humans Review (and What They Don't)

```
HUMANS REVIEW:                          AI ALREADY HANDLED:
─────────────────                       ────────────────────
Does the contract match the outcome?    Does the code match the contract?
Is the decomposition still right?       Does it compile?
Are the boundaries in the right place?  Do tests pass?
Did we miss an edge case that matters   Code style and conventions?
  to the business?                      Security vulnerabilities?
Is the shared glossary still accurate?  Test coverage?
Should we pivot or adjust the outcome?  Documentation accuracy?
```

---

## Review Cadence

Three types of alignment review, at different frequencies:

### 1. Contract Review (per unit, before generation)

Before AI generates code, a human reviews the contract:

```
Checklist:
- [ ] Does each rule trace to the opportunity tree?
- [ ] Would a customer recognize these behaviors as solving their problem?
- [ ] Are the anti-goals from Layer 1 respected?
- [ ] Are the invariants actually invariant? (not just "usually true")
- [ ] Is anything missing that would surprise a user?
```

### 2. Integration Review (per dependency connection)

When units connect, review the seams:

```
Checklist:
- [ ] Does the producer's output contract match the consumer's input contract?
- [ ] Are failure modes at boundaries handled explicitly?
- [ ] Does the combined behavior match what a user would expect end-to-end?
- [ ] Does the integration introduce emergent behavior not in any single contract?
```

### 3. Outcome Review (periodic, against metrics)

Step back and check the whole system against Layer 1:

```
Checklist:
- [ ] Are the leading metrics moving in the right direction?
- [ ] Has anything changed in the business context that invalidates our outcome?
- [ ] Should we prune or add branches to the opportunity tree?
- [ ] Are we building units that don't connect to any outcome? (waste)
```

---

## The Review Artifact

Reviews produce one of three outputs:

```
ALIGNED     → Proceed. No changes needed.
ADJUST      → Contract needs modification. Return to Layer 3.
             AI regenerates from updated contract.
PIVOT       → Outcome or decomposition has shifted. Return to Layer 1 or 2.
             Larger scope change.
```

---

## Ownership Analysis

### What's Actually Happening When a Human Reviews

```
1. Compare implementation to contract     → "Does the code do what the spec says?"
2. Compare contract to intent             → "Does the spec say what we meant?"
3. Compare intent to outcome              → "Does what we meant actually serve the goal?"
4. Spot emergent behavior                 → "These units together create behavior nobody specified"
5. Apply common sense                     → "A user would never do this, even though it's technically allowed"
```

### What Would AI Need to Own This

| Sub-task | Barrier | Type |
|---|---|---|
| Implementation vs. contract | AI can do this today — it's formal verification | **Already AI-capable** |
| Contract vs. intent | Requires a formalized notion of "intent" to verify against | **Chicken-and-egg** — if intent is formalized, it's just another contract |
| Intent vs. outcome | Requires understanding causal chains from features to business metrics | **Hard but approachable** — with good instrumentation |
| Spot emergent behavior | Requires reasoning about system-level behavior from unit-level specs | **Current AI limitation** — but improving with longer context and better reasoning |
| Apply common sense | Requires understanding human behavior, social norms, expectations | **Fundamental gap** — AI lacks lived experience |

### The Honest Assessment

**This layer has the most genuinely irreducible human element.**

The alignment verification problem is recursive. To verify alignment, you need a reference for "what we meant." But if "what we meant" is perfectly formalized, it's just another spec — and you need to verify *that* spec against an even higher-level intent. Eventually you bottom out at **human judgment about what feels right**, which isn't formalizable.

However, AI can do enormous amounts of *pre-screening*:

```
AI CAN flag: "Contract says no reminders during quiet hours,
but your outcome metric counts ALL reminders sent. Are quiet-hour
users being excluded from your success measurement?"

AI CANNOT judge: "Is it acceptable to slightly annoy users
during quiet hours if it reduces missed appointments by an
additional 3%?"
```

The first is logical analysis. The second is a values trade-off.

---

## Irreducible Human Input

```
IRREDUCIBLE:
├── "Yes, this is what I meant" (final confirmation)
├── Values trade-offs: "Annoy users slightly vs. miss the target"
├── Common sense checks: "A real person would never do this"
├── Emergent behavior judgment: "These units together feel wrong"
└── Strategic coherence: "This technically works but contradicts our brand"

AI-GENERATABLE:
├── Contract-to-implementation verification
├── Cross-contract consistency checking
├── Traceability gap identification
├── Anomaly flagging ("this edge case isn't covered by any contract")
├── Test coverage analysis against contracts
└── Risk scoring ("this unit has the most unreviewed changes")
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Contract-to-implementation check | Reviews AI's report | Performs verification |
| Contract-to-intent check | Judges | Flags inconsistencies with Layer 1 |
| Cross-contract consistency | Reviews flags | Checks all contracts against each other |
| Emergent behavior detection | Judges system-level behavior | Simulates unit interactions |
| Common sense review | Applies lived experience | Cannot substitute |
| Values trade-offs | Decides | Frames the trade-off clearly |
| Review scheduling | Sets cadence | Flags units needing review based on risk |
