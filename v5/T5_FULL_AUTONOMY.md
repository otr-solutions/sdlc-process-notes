# T5: Full AI Autonomy

> **AI operates without human involvement. Escalation only on anomaly.**

## When to Use

```
├── The activity is COMMODITY (zero strategic value)
├── The activity is FULLY UNDERSTOOD (no ambiguity)
├── Failure is AUTOMATICALLY DETECTABLE and RECOVERABLE
├── The activity has been at T4 with ZERO ISSUES for 6+ months
└── Industry-standard automation exists
```

## Activities at This Level

```
├── Build pipeline execution
├── Log aggregation and rotation
├── Uptime and health monitoring
├── Backup and restore procedures
├── Certificate renewal
├── CDN cache management
├── Infrastructure scaling (within preset bounds)
└── Automated alerting (PagerDuty, etc.)
```

## How It Works

```
1. AI operates fully autonomously
2. No human review, no audits (unless triggered)
3. Self-healing: AI detects failures and recovers automatically
4. ONLY escalation trigger: anomaly that AI cannot self-resolve
```

## Why T5 Exists

Some activities have zero alignment content. No amount of human attention improves certificate renewal or log rotation. Human involvement at T5 is **pure waste**.

T5 exists to **explicitly free human attention** from these activities. Making T5 explicit prevents the anti-pattern of humans "keeping an eye on" commodity operations that never need attention.

---

## The T5 Contract

Each T5 activity has a minimal contract:

```
ACTIVITY:     Build pipeline execution
TRIGGER:      Code merged to main
ACTION:       Run full pipeline (build, test, deploy to staging)
SUCCESS:      All gates pass, deployed to staging
FAILURE:      Retry once. If second failure, escalate to T4.
ANOMALY:      Pipeline takes >3x average time → escalate to T4
MONITORING:   Pipeline success rate dashboard (T4 audit reviews this)
```

## Risk: The Automation Complacency Trap

```
DANGER:  Nobody understands T5 activities anymore.
         When a T5 activity fails in a novel way, nobody knows how to fix it.

MITIGATION:
  - Document T5 activities thoroughly (AI maintains docs)
  - Ensure at least one team member has context (bus factor ≥ 2)
  - Periodic chaos engineering: intentionally break T5 activities
    to verify recovery procedures
  - T5 activities have runbooks (AI-maintained, human-reviewed annually)
```

---

## Demotion Criteria (T5 → T4)

```
├── Novel failure that AI couldn't self-resolve
├── Domain change (new compliance requirements, etc.)
├── Nobody on the team understands the activity anymore
├── The activity starts having alignment implications it didn't before
└── Annual review reveals stale runbooks or monitoring gaps
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Operate | Not involved | Full autonomy |
| Monitor | Not involved (unless escalated) | Self-monitors |
| Recover from failure | Only if AI can't self-resolve | Self-heals within contract |
| Annual review | Reviews runbooks and contracts | Maintains documentation |
| Chaos testing | Initiates periodically | Executes and reports |
