# Station 4: MAKE

> **"Build it, verify it, trace it."**

## Role in the Flow

MAKE receives validated specs and generates **all derivable artifacts**: implementation, tests, documentation, and traceability. This is the only station with no WIP limit — because it's not the constraint.

### Pull Relationship
- MAKE pulls from SPEC: "I need a validated spec to generate from."
- Nothing pulls from MAKE except the validation spine (V-Model pairs)

MAKE is the **terminus of the flow**. Work exits the system here as validated, traceable, deployed software.

---

## WIP Limit: None

MAKE is AI-driven. AI can generate multiple units in parallel. The constraint is upstream (human alignment decisions), not here. Limiting WIP here would be Goldratt's cardinal sin: optimizing a non-constraint.

---

## V-Model Pairing: Unit Validation

```
MAKE ◄──────────────────────────────────► UNIT VALIDATION

"Generated code, tests, docs"            "Compiles? Tests pass? Coverage?
                                           Security scan? Conventions?"
```

Unit Validation is **fully automated**. No human in the loop unless gates fail repeatedly (which signals a spec problem, not a generation problem).

---

## Model-Driven Architecture: PIM → PSM

MAKE performs the Platform-Independent → Platform-Specific transformation:

```
Input (PIM/Spec):                    Output (PSM/Code):
─────────────────                    ────────────────────
"First reminder 24hrs before"    →   scheduledFor = appointmentTime
                                       .subtract(24, 'hours')

"No reminders during quiet hrs"  →   if (isWithinQuietHours(time, prefs))
                                       time = nearestQuietHourEdge(...)

"Channel: first in preference"   →   const channel = prefs.channels
                                       .find(c => isValid(c, user))
```

The spec (PIM) is stable. The platform choice (PSM) is a configuration decision. Changing from TypeScript to Go means re-running the transformation, not re-thinking the spec.

---

## What Gets Generated

### 1. Implementation
```
Context loaded:
  - Intent card (~2K tokens)
  - Glossary + schemas (~5K tokens)
  - Unit spec (~8K tokens)
  - Total input: ~15K tokens
  - Remaining for generation + reasoning: ~45-50K tokens ✓

Prompt: "Implement the reminder-scheduling unit per its spec.
Use TypeScript. Follow project conventions. Every rule must be
implemented. Every invariant must be enforced."
```

### 2. Tests (generated independently from spec, not from code)

```
Test categories:
├── Happy path    (from acceptance criteria)
├── Edge cases    (from edge cases section)
├── Invariants    (property-based tests from invariants)
├── Boundaries    (threshold values from rules)
└── Contracts     (input/output schema validation)
```

The Cleanroom principle: **tests and code are derived from the same spec but generated independently**. If they agree, confidence is high. If they disagree, the spec is the tiebreaker.

### 3. API Documentation
Generated from spec inputs/outputs/rules. Not hand-written.

### 4. Traceability
```
Intent: Reduce missed appointments <10%
  → Unit: reminder-scheduling
    → Spec: reminder-scheduling.spec.md (Rule 1)
      → Code: src/reminder-scheduling/index.ts:L42-L58
        → Test: tests/reminder-scheduling/24hr-reminder.test.ts
```

### 5. Change Impact Analysis
When a spec changes:
- Which code needs regeneration?
- Which tests are affected?
- Which downstream units depend on changed outputs?
- Is the traceability chain intact?

---

## Automated Quality Gates

```
Gate 1: Compile / type-check                     [automated]
Gate 2: All generated tests pass                  [automated]
Gate 3: Invariants hold under fuzzing             [automated]
Gate 4: Test coverage ≥ threshold                 [automated]
Gate 5: Security scan (SAST, dependencies)        [automated]
Gate 6: Project conventions (lint, format)        [automated]
Gate 7: Spec compliance (AI verifies vs. spec)    [automated]
```

All seven gates are automated. **No human in the loop for correctness.** Humans only enter at the V-Model validation levels above Unit (Integration and Outcome validation — which live in SHAPE and INTENT's validation pairs).

---

## Regeneration vs. Editing

### The Regeneration Principle

**Prefer regeneration over editing.** When a spec changes:

```
PREFERRED:
  Spec v1.0 → Generated Code v1.0
  Spec v1.1 → Generated Code v1.1 (full regeneration)

AVOIDED:
  Spec v1.0 → Generated Code v1.0
  Spec v1.1 → Generated Code v1.0 + manual patch (drift risk)
```

Regeneration prevents drift between spec and code. It's only feasible because generation is cheap — which it is, because that's the whole premise of V2.

### When Regeneration Isn't Practical
- Performance-critical code with hand-tuned optimizations
- Complex third-party integrations with brittle configurations
- Code with accumulated runtime-discovered edge cases

In these cases, AI generates a **diff recommendation** and a human reviews. These units should be flagged as "regeneration-resistant" and given extra attention in Integration Validation.

---

## PDCA Micro-Cycle

```
PLAN:  Load context: intent card + glossary + schemas + spec
       Verify total context fits within budget

DO:    AI generates implementation + tests + docs
       All generated independently from the same spec

CHECK: Run all 7 automated gates
       If all pass → unit is done
       If gates fail → diagnose

ACT:   Gate failure diagnosis:
       - Compile error → AI fixes and regenerates
       - Test failure → AI fixes (is the test wrong or the code?)
       - Repeated failures → SPEC may be ambiguous, push back upstream
```

---

## Flow Metrics at MAKE

```
Generation Time:      Time from spec pull to gates passing
                      Target: minutes, not hours

Gate Pass Rate:       % of generations that pass all gates on first attempt
                      High rate = specs are clear. Low rate = spec quality problem.

Regeneration Rate:    % of code that is regenerated (not hand-edited)
                      Target: >85%. Lower = drift risk.

Spec Pushback Rate:   % of specs returned to SPEC station due to ambiguity
                      Target: <10%. Higher = SPEC clarity test needs improvement.
```

---

## Ownership

### Theory of Constraints View

MAKE is the **non-constraint**. Goldratt's Rule 3: subordinate everything to the constraint. MAKE subordinates to alignment:

- MAKE doesn't start without a validated spec (subordination to SPEC)
- MAKE doesn't define what to build (subordination to INTENT)
- MAKE doesn't decide boundaries (subordination to SHAPE)
- MAKE does **one thing**: transform validated specs into verified artifacts, as fast as possible

### Who Does What

| Activity | Human | AI |
|---|---|---|
| Generate implementation | Spot-checks | Authors from spec |
| Generate tests | Spot-checks | Authors independently from spec |
| Generate documentation | Skims | Authors from spec + code |
| Run quality gates | Reviews failures only | Executes all gates |
| Maintain traceability | Audits periodically | Updates continuously |
| Change impact analysis | Reviews recommendations | Identifies affected artifacts |
| Regeneration decisions | Approves when flagged | Proposes regen vs. edit |

### Irreducible Human Input

```
├── Spot-check: "Does this look reasonable?" (rare, sampling-based)
├── Override: "Use this specific library/approach" (exception, not norm)
├── Regeneration-resistant units: Review diffs manually
└── Gate failure escalation: When AI can't self-fix
```

MAKE is the station with the **least irreducible human input**. By design. The constraint is elsewhere.
