# Domain 3: BANDWIDTH

> **"How much alignment information can flow between nodes?"**

## Purpose

Communication channels have limited capacity. Human-to-human bandwidth is low (meetings, documents, conversations). Human-to-AI bandwidth is medium (context windows, prompts). AI-to-AI bandwidth is high (shared artifacts). Optimize the network to push information through the highest-bandwidth channels.

---

## Bandwidth by Channel

```
CHANNEL                      BANDWIDTH        LATENCY         FIDELITY
──────────────               ─────────        ───────         ────────
Human ←→ Human (meeting)     Very Low         Real-time       High (with body language)
Human ←→ Human (document)    Low              Hours-days      Medium (words are ambiguous)
Human ←→ Human (Slack)       Low              Minutes         Low (context-free, fragmented)
Human → AI (prompt+context)  Medium           Seconds         Medium-High (structured context)
AI → Human (output)          Medium-High      Seconds         Medium (needs review)
AI ←→ AI (shared artifacts)  Very High        Instant         High (structured, machine-readable)
```

### The Bandwidth Optimization Principle

**Route information through the highest-bandwidth channel that preserves fidelity.**

```
INSTEAD OF:                          DO THIS:
─────────────                        ─────────
Team A explains to Team B in meeting Team A writes spec; AI loads it for Team B's AI
  → Low bandwidth, high latency        → High bandwidth, instant

Dev A reviews Dev B's code line by line AI reviews for correctness; Dev A reviews alignment only
  → Low bandwidth on low-value check    → Human bandwidth spent on high-value check

PM emails requirements to 5 developers PM writes outcome doc; AI loads it into every dev context
  → N copies, each slightly different   → Single source of truth, machine-distributed
```

---

## Brooks's Law Mitigation

Brooks showed communication overhead scales quadratically: N people = N(N-1)/2 channels. AI can flatten this:

```
WITHOUT AI HUB:              WITH AI HUB:
───────────────               ─────────────
5 people = 10 channels        5 people = 5 channels (each to AI)
10 people = 45 channels       10 people = 10 channels
20 people = 190 channels      20 people = 20 channels

AI acts as a hub: translating, summarizing, and routing.
Each human maintains ONE channel to AI.
AI maintains coherence across all channels via shared artifacts.
```

This doesn't eliminate the need for human-to-human communication — but it reduces it to alignment-critical conversations only.

---

## Context Window as Bandwidth Constraint

The context window IS the bandwidth of the human-AI channel:

```
TOTAL BANDWIDTH:       ~128K tokens per session
FIXED OVERHEAD:        ~10K tokens (shared context, glossary, schemas)
AVAILABLE PER TASK:    ~50-60K tokens (after overhead and reasoning room)

BANDWIDTH OPTIMIZATIONS:
├── Compress: Use summaries instead of full artifacts when possible
├── Prioritize: Load highest-signal artifacts first
├── Trim: Remove stale or irrelevant context before loading
├── Cache: Shared context is "cached" — same across sessions
└── Stream: For large tasks, process in sequential context windows
```

---

## Bandwidth Allocation by Activity

```
HIGH BANDWIDTH ALLOCATION (worth the context tokens):
├── Active unit's spec: High signal, directly needed
├── Outcome definition: Keeps generation aligned
├── Interface contracts: Prevents boundary violations
└── Recent decision records: Prevents rehashing decisions

LOW BANDWIDTH ALLOCATION (compress or omit):
├── Other units' implementations: Not needed (information hiding)
├── Historical specs: Only relevant if actively referenced
├── Meeting notes: Summarize to 3 sentences or omit
└── Process documentation: Embed in conventions, not context
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Choose communication channels | Decides what's async vs. sync | Proposes based on bandwidth analysis |
| Optimize context loading | Validates what's included/excluded | Calculates signal value per token |
| Mediate between teams | Handles relationship/political issues | Translates and routes information via artifacts |
| Reduce meeting overhead | Decides which meetings are necessary | Generates summaries, pre-reads, and agendas |
| Monitor bandwidth utilization | Reviews whether communication works | Tracks context utilization, identifies waste |
