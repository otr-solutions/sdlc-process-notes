# Phase 3: DECODE

> **"Interpret the signal and act on it — without introducing new distortion."**

## Purpose

Decoding is where the received signal becomes action: AI generates code from a spec, a human interprets metrics, a team reads a design doc. Every act of interpretation is a potential distortion point. DECODE optimizes for faithful interpretation.

---

## Decoding Contexts

### AI Decoding a Spec → Code

```
RISK: AI interprets ambiguous spec language using training distribution
      (what most code looks like) instead of spec intent (what THIS code should do)

EXAMPLE:
  Spec says: "Channel selection: use first available in user's preference order"
  AI might decode as: "iterate array, return first non-null"
  But intent might be: "first available AND deliverable right now
                        (SMS provider might be down)"

MITIGATION:
  - Encode with concrete examples (ENCODE phase)
  - Test against spec, not against code (independent verification)
  - Clarity test catches ambiguity before generation
```

### Human Decoding Metrics → Decisions

```
RISK: Humans see patterns in noise (apophenia) or miss signal in data

EXAMPLE:
  Metric shows: no-show rate dropped from 23% to 18%
  Optimistic decode: "It's working! Keep going!"
  Careful decode: "Is this seasonal? Is the sample large enough?
                   Did anything else change simultaneously?"

MITIGATION:
  - AI provides statistical context (confidence intervals, baselines)
  - Pre-defined decision criteria prevent emotional interpretation
  - Multiple metrics triangulate (if they all point the same way, it's signal)
```

### Human Decoding AI Output → Alignment Judgment

```
RISK: Automation bias — accepting AI output because reviewing is effortful

EXAMPLE:
  AI generates a contract with 7 rules. All look reasonable.
  But Rule 4 subtly contradicts the anti-goals from Layer 1.
  Tired reviewer approves without catching it.

MITIGATION:
  - Structured review checklists (from V5 T2/T3)
  - AI pre-screens for contradictions (before human review)
  - Random deep-dive audits (catch what routine review misses)
```

---

## Faithful Decoding Principles

### 1. Decode Against the Source, Not Against Expectations

```
BAD:  "Does this code look right?" (compared to what I expect)
GOOD: "Does this code match the spec?" (compared to the encoded signal)

The spec is the source of truth. Human expectations may have drifted.
```

### 2. Separate Decoding from Judgment

```
STEP 1 (DECODE):  "What does this output actually do?"
STEP 2 (JUDGE):   "Is what it does aligned with intent?"

Conflating these leads to: "It looks fine" (skipped step 1)
Separating them forces: "It does X. Is X what we wanted?"
```

### 3. Verify Decoding Fidelity

```
TECHNIQUE: Round-trip test

1. Encode intent → spec
2. AI decodes spec → code
3. AI re-encodes code → description
4. Compare description to original intent

If description ≠ intent → signal was lost somewhere in the chain
This is the software equivalent of Shannon's error detection.
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Decode specs into code | Spot-checks | Generates implementation |
| Decode metrics into meaning | Interprets with context | Provides statistical analysis |
| Decode AI output into alignment judgment | Judges against intent | Pre-screens for contradictions |
| Round-trip verification | Reviews discrepancies | Executes encode-decode-re-encode cycle |
| Detect decoding bias | Self-monitors for automation bias | Flags suspiciously fast reviews |
