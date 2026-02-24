# Phase 4: CORRECT

> **"Detect and fix signal degradation before it compounds."**

## Purpose

Signal degradation is inevitable. CORRECT builds **error detection and correction mechanisms** into the SDLC — analogous to Hamming codes in data transmission. The goal isn't to prevent all errors, but to **detect them before they compound**.

---

## Error Detection Mechanisms

### 1. Parity Checks (Quick, Cheap, Frequent)

Simple checks that catch obvious degradation:

```
SPEC PARITY:    Does every spec rule have a corresponding test?
                Missing test = potential undetected error

TRACE PARITY:   Does every unit trace to an outcome?
                Untraced unit = potential wasted work

GLOSSARY PARITY: Is every domain term used consistently?
                  Inconsistent usage = semantic noise

BUDGET PARITY:  Does every unit fit in its context budget?
                Over-budget = degradation in AI generation quality
```

AI runs parity checks continuously. Failures are flagged immediately.

### 2. Checksums (Periodic, Moderate Depth)

Deeper checks that verify structural integrity:

```
CONTRACT CHECKSUM:
  Feed spec to AI → generate implementation → generate spec from implementation
  Compare generated spec to original spec
  Differences = signal lost in generation

INTEGRATION CHECKSUM:
  Combine two units' interface contracts
  Check: are types compatible? Are error cases handled at boundaries?
  Incompatibilities = boundary signal degradation

OUTCOME CHECKSUM:
  Compare current metrics to Layer 1 targets
  Divergence without explanation = alignment drift
```

### 3. Full Verification (Rare, Expensive, Comprehensive)

Complete signal chain audit:

```
FULL CHAIN VERIFICATION:
  Intent → Outcome statement → Specs → Code → Tests → Production behavior

  At each step, verify:
    1. Forward: Does downstream faithfully represent upstream?
    2. Backward: Can you reconstruct upstream from downstream?
    3. Lateral: Are parallel paths consistent with each other?

  This is expensive (human time). Do it:
    - At major milestones
    - When metrics are unexpectedly off
    - When the team "feels" something is wrong but can't articulate it
```

---

## Error Correction Strategies

### 1. Regeneration (Strongest Correction)

When code has drifted from spec, don't patch — **regenerate from spec**:

```
DETECTED:  Implementation doesn't match 2 of 7 spec rules
WRONG FIX: Patch the implementation to match
RIGHT FIX: Regenerate entirely from spec

WHY: Patching introduces its own errors. Regeneration resets to source signal.
```

### 2. Spec Update (Source Correction)

When the spec itself is wrong — the source signal was bad:

```
DETECTED:  Spec says X, but users actually need Y
WRONG FIX: Change the code to do Y (spec still says X → future drift)
RIGHT FIX: Update spec to say Y, then regenerate code

WHY: The spec is the source of truth. Correcting anywhere else is temporary.
```

### 3. Glossary Correction (Semantic Correction)

When terms are used inconsistently:

```
DETECTED:  "Appointment" means different things in 3 specs
WRONG FIX: Add clarifying comments in each spec
RIGHT FIX: Update glossary with precise definition; regenerate affected specs

WHY: Semantic noise propagates. Fix it at the source (glossary).
```

---

## Redundancy as Error Detection (Hamming Principle)

Hamming showed that **structured redundancy** enables error detection and correction. In SDLC:

```
REDUNDANCY TYPE             PURPOSE
─────────────               ───────
Tests mirror spec rules     Detect when code diverges from spec
Traceability mirrors        Detect when work diverges from outcome
  outcome tree
Glossary mirrors domain     Detect when language diverges from meaning
  understanding
AI re-generates spec        Detect when code has drifted from intent
  from code (round-trip)
```

This isn't "wasteful duplication" — it's structured redundancy that enables error detection. Shannon proved you can't have reliable communication without it.

---

## Correction Metrics

```
Error Detection Rate:   % of signal degradation caught before production
                        Target: >95%

Mean Time to Detection: How long between signal degradation and detection
                        Target: <1 sprint (catch within the iteration)

Correction Source:      Where do corrections happen?
                        At spec level: GOOD (source correction)
                        At code level: BAD (symptomatic fix)
                        At production: TERRIBLE (signal reached users corrupted)

False Positive Rate:    % of flagged issues that aren't real problems
                        Target: <20% (too many false alarms = alert fatigue)
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Run parity checks | Reviews failures | Executes continuously |
| Run checksums | Reviews periodically | Executes on schedule |
| Full chain verification | Leads comprehensive audit | Prepares data, identifies discrepancies |
| Choose correction strategy | Decides (regenerate vs. patch vs. spec update) | Recommends based on degradation type |
| Execute corrections | Approves spec changes | Regenerates code, updates traceability |
| Monitor correction metrics | Judges whether system is healthy | Tracks and trends all metrics |
