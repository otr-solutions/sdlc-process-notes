# V7: Signal-Preserving Development

> **Core Question: "Where does alignment signal degrade — and how do we preserve it?"**

## The Philosophical Difference

- V1-V6 treat alignment loss as a process or organizational problem
- V7 treats alignment loss as an **information theory problem** — signal degrades through noisy channels, and every handoff is a potential corruption point

## Why This Dimension Matters

Shannon proved that information degrades when transmitted through noisy channels. In software development, "intent" is the signal and every handoff — human to human, human to AI, AI to code — is a noisy channel. **Misalignment is entropy**: the natural tendency of signal to degrade without active error correction.

---

## Foundation Models

| Model | Era | Core Insight | V7 Application |
|---|---|---|---|
| Information Theory (Shannon, 1948) | 40s | Signals degrade through noisy channels; redundancy enables error correction | Every SDLC handoff is a noisy channel; build in error correction |
| SECI Model (Nonaka, 1995) | 90s | Knowledge converts between tacit and explicit through four modes | Alignment lives as tacit knowledge; must be made explicit to survive |
| Signal Detection Theory (Green & Swets, 1966) | 60s | Distinguish true signals from noise with decision criteria | Filter alignment-relevant signals from SDLC noise |
| Error-Correcting Codes (Hamming, 1950) | 50s | Add structured redundancy to detect and correct errors | Build redundancy into alignment artifacts |

---

## The V7 Model: Four Signal Phases

```
┌─────────────────────────────────────────────────────────┐
│            SIGNAL-PRESERVING DEVELOPMENT                 │
│                                                          │
│  ┌────────────┐   ┌────────────┐                         │
│  │  ENCODE    │──→│  TRANSMIT  │                         │
│  │            │   │            │                         │
│  │ "Convert   │   │ "Move      │                         │
│  │  intent    │   │  through   │                         │
│  │  into      │   │  channels" │                         │
│  │  signal"   │   │            │                         │
│  └────────────┘   └─────┬──────┘                         │
│       ▲                 │                                │
│       │                 ▼                                │
│  ┌────────────┐   ┌────────────┐                         │
│  │  CORRECT   │◄──│   DECODE   │                         │
│  │            │   │            │                         │
│  │ "Fix       │   │ "Interpret │                         │
│  │  degraded  │   │  and       │                         │
│  │  signal"   │   │  act"      │                         │
│  └────────────┘   └────────────┘                         │
│                                                          │
│  Cycle: Encode → Transmit → Decode → Correct → Encode   │
└─────────────────────────────────────────────────────────┘
```

---

## Alignment as Signal

### The Signal Chain

```
BUSINESS INTENT (pure signal)
    │
    ▼ ── Encode ──→ Outcome statement (some loss)
    │
    ▼ ── Transmit ──→ Team reads document (more loss)
    │
    ▼ ── Encode ──→ Spec/contract (more loss)
    │
    ▼ ── Transmit ──→ AI context window (more loss)
    │
    ▼ ── Decode ──→ Generated code (more loss)
    │
    ▼ ── Transmit ──→ Production (more loss)
    │
    ▼
USER EXPERIENCE (received signal — how much of the original survives?)
```

Every arrow is a **noisy channel**. The question is: how much signal survives from intent to experience?

### Types of Noise

```
SEMANTIC NOISE:    Words mean different things to different people
                   "Appointment" means one thing to scheduling, another to billing

COMPRESSION NOISE: Simplifying loses nuance
                   Full intent = 10 pages; spec = 2 pages; what was lost?

TRANSLATION NOISE: Converting between formats introduces errors
                   Business rule → code → test → each translation can distort

TEMPORAL NOISE:    Context changes over time; artifacts become stale
                   Spec written in January; market shifted in March

ATTENTION NOISE:   Reviewer misses something because of cognitive overload
                   20th spec review of the day — fatigue degrades review quality
```

---

## The SECI Model: Tacit ↔ Explicit

Nonaka showed knowledge exists in four modes. Alignment signal lives in all four:

```
                    TACIT                      EXPLICIT
                    (in people's heads)        (in artifacts)
                    
TO TACIT:           SOCIALIZATION              INTERNALIZATION
                    Human ←→ Human             Artifact → Human understanding
                    "Pairing to share           "Reading the spec and truly
                     intuition about users"      understanding the intent"

TO EXPLICIT:        EXTERNALIZATION            COMBINATION
                    Human → Artifact           Artifact → Artifact
                    "Writing down what I        "Generating code from spec;
                     know about the domain"      combining specs into system"
```

### The Critical Conversion: Externalization

The highest-risk conversion is **tacit → explicit** (externalization). This is where the most signal is lost:

```
TACIT (in stakeholder's head):
  "Users get frustrated when they can't easily move their appointment.
   They're not lazy — they're busy. They need it to feel effortless.
   One tap. No login screens. No 'are you sure?' dialogs."

EXPLICIT (in spec):
  "Users can reschedule with one tap from the reminder notification."

SIGNAL LOST:
  - The emotional context ("frustrated," "effortless")
  - The design constraint ("no login screens, no confirmation dialogs")
  - The empathy framing ("not lazy — busy")
```

AI can't recover what was never externalized. **The quality of the externalization determines the ceiling for AI's output quality.**

---

## Context Windows as Channels

The context window is literally a Shannon channel:

```
CHANNEL CAPACITY:     ~128K tokens
SIGNAL:               Specs, contracts, outcome definition
NOISE:                Irrelevant artifacts, stale context, ambiguous language
BANDWIDTH:            Effective tokens that carry alignment signal

SHANNON'S THEOREM:    Channel capacity limits error-free transmission rate

IMPLICATION:          You can't load infinite context and expect good output.
                      Pack the channel with signal, not noise.
                      Use error correction (review, testing) to catch degradation.
```

---

## Comparison

| Dimension | Key Insight |
|---|---|
| V1 (Structure) | Build the right artifacts |
| V2 (Flow) | Move work through efficiently |
| V3 (Cognitive) | Spend attention wisely |
| V4 (Risk) | Bet on the right things |
| V5 (Trust) | Delegate appropriately |
| V6 (Communication) | Connect people effectively |
| **V7 (Information)** | **Preserve signal through every handoff** |

→ See phase documents: [Encode](ENCODE.md) · [Transmit](TRANSMIT.md) · [Decode](DECODE.md) · [Correct](CORRECT.md)
