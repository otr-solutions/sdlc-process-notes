# Journey Map

> **"How does value flow from user need to delivered outcome?"**

## Purpose

Traces the path from a user's problem to the solution, step by step. Each step is an alignment checkpoint: is the value surviving this transition?

## How to Build

```
1. Start with the user's problem (not your feature)
2. Walk through every step the user takes
3. At each step, ask:
   - What does the user need to succeed here?
   - What could go wrong?
   - Which system component serves this step?
   - How do we know this step is working?
```

## Example

```
JOURNEY: "I might miss my appointment"

STEP 1: Book appointment
  User needs: Easy booking, clear confirmation
  Component: Booking API (existing, stable)
  Risk: Low (this step works today)
  Metric: Booking completion rate

STEP 2: Receive reminder
  User needs: Right channel, right time, clear message
  Component: reminder-scheduling (building this)
  Risk: HIGH — core assumption being tested
  Metric: Reminder delivery rate, open rate

STEP 3: Act on reminder
  User needs: One-tap confirm or reschedule
  Component: reschedule-flow (building this)
  Risk: HIGH — is the UX friction low enough?
  Metric: Action rate (confirm + reschedule)

STEP 4: Attend (or successfully reschedule)
  User needs: Nothing goes wrong between action and appointment
  Component: System reliability, calendar accuracy
  Risk: Medium (integration between systems)
  Metric: No-show rate (the outcome metric)
```

## Alignment Insight

The journey map reveals **where the outcome is actually decided**:

```
If Step 2 (receive) works but Step 3 (act) fails:
  → The problem isn't reminders. It's the reschedule UX.
  → Reallocate attention from reminder-scheduling to reschedule-flow.

If Step 2 (receive) fails:
  → Channel selection or delivery is the bottleneck.
  → Focus on notification-delivery, not new features.
```

The journey map prevents the classic error: **optimizing one step while another step is the actual bottleneck.**

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define journey steps | Authors from user research | Suggests from usage data |
| Identify risks per step | Judges from domain knowledge | Flags from error rates and metrics |
| Map components to steps | Validates | Derives from traceability |
| Detect bottleneck steps | Interprets metrics | Identifies steps with highest drop-off |
| Update journey as product evolves | Reviews | Tracks metric changes per step |
