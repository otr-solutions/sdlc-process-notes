# Phase 1: ENCODE

> **"Convert tacit intent into structured, transmittable signal."**

## Purpose

Transform what's in people's heads into artifacts that can be loaded into AI context windows, shared across teams, and verified for alignment. This is the highest-risk phase — once signal is lost in encoding, no downstream process can recover it.

---

## Encoding Strategies

### 1. Structured Templates (Reduce Compression Noise)

Templates force completeness. Without them, people externalize what's top-of-mind and forget the rest.

```
OUTCOME TEMPLATE:
  What changes? _______________
  For whom? _______________
  How measured? _______________
  What NOT to do? _______________

CONTRACT TEMPLATE:
  Purpose: _______________
  Inputs: _______________
  Outputs: _______________
  Rules (≤7): _______________
  Edge cases: _______________
  Invariants: _______________
```

Every blank field is a prompt for tacit knowledge that might otherwise stay tacit.

### 2. Ubiquitous Language (Reduce Semantic Noise)

Maintain a **glossary** — a shared vocabulary that eliminates ambiguity:

```
TERM          MEANS                           DOES NOT MEAN
────          ─────                           ─────────────
Appointment   A scheduled meeting between     A calendar entry (those are
              patient and provider            "events")

Reminder      A notification sent before      A follow-up after a missed
              an appointment                  appointment (those are "outreach")

Quiet Hours   User-defined time range         System maintenance windows
              when notifications are          (those are "blackout periods")
              suppressed
```

The glossary loads into every AI context window. It's **error correction for semantic noise** — ensuring the same word means the same thing everywhere.

### 3. Concrete Examples (Reduce Abstraction Noise)

Abstract rules lose nuance. Concrete examples preserve it:

```
ABSTRACT (lossy):
  "Reminders should not be sent during quiet hours."

CONCRETE (preserves signal):
  "If quiet hours are 10pm-8am and appointment is at 9am:
   - The 24hr reminder (normally 9am day before) is sent at 9am ✓
   - The 2hr reminder (normally 7am) is delayed to 8am (quiet hours end)
   - Result: user gets woken up at 8am, not 7am"
```

### 4. Negative Examples (Reduce Omission Noise)

State what the system should NOT do. Anti-examples carry as much signal as positive examples:

```
THE SYSTEM MUST NOT:
  - Send a reminder after the appointment has passed (even if "scheduled")
  - Send duplicate reminders for the same appointment + channel
  - Override user quiet hours for any reason (not even "urgent" reminders)
```

---

## Encoding Quality Metric

**Clarity Score**: Feed the encoded artifact to AI and count clarifying questions:

```
0 questions:  Signal is clear. AI can act without ambiguity.
1-2 questions: Minor gaps. Usually edge cases or implicit assumptions.
3+ questions:  Significant signal loss. Re-encode with more specificity.
```

This metric is objective and repeatable — making encoding quality measurable.

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Externalize tacit knowledge | Authors using templates | Asks probing questions to extract more |
| Build glossary | Defines authoritative meanings | Flags terms used inconsistently |
| Create examples | Authors from domain experience | Generates additional examples from rules |
| Create negative examples | Authors from knowing what's wrong | Enumerates edge cases that might violate rules |
| Measure encoding quality | Reviews clarity score | Executes clarity test |
