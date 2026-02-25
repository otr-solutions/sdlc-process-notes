# T4: AI Autonomous with Guardrails

> **AI operates independently within defined constraints. Human audits periodically.**

## When to Use

```
├── The activity is ROUTINE and WELL-UNDERSTOOD
├── AI has a LONG TRACK RECORD with near-zero errors
├── Automated gates are COMPREHENSIVE and RELIABLE
├── The cost of an undetected error is LOW or RECOVERABLE
└── Human review consistently adds NO VALUE
```

## Activities at This Level

```
├── Code formatting and linting
├── Security scanning (SAST, dependency audit)
├── Dependency version management (minor/patch)
├── Metric collection and dashboarding
├── Spec clarity scoring
├── Context window budget monitoring
├── Documentation regeneration on spec change
├── Traceability matrix maintenance
└── Build pipeline execution
```

## How It Works

```
1. Human defines GUARDRAILS (boundaries that trigger escalation)
2. AI operates within guardrails without human involvement
3. If guardrails are breached → ESCALATE to T3 or T2
4. Human audits periodically (weekly/monthly sampling)
```

### Guardrail Design

Guardrails define the **boundary of autonomous operation**:

```
GUARDRAIL TYPES:

Threshold guardrails:
  "If test coverage drops below 80%, escalate"
  "If context window utilization exceeds 75%, escalate"
  "If security scan finds critical severity, escalate"

Scope guardrails:
  "Only update patch versions, not minor or major"
  "Only modify files within the unit boundary"
  "Never modify shared contracts without escalation"

Pattern guardrails:
  "If AI asks >2 clarifying questions, escalate"
  "If generated code differs >40% from previous version, escalate"
  "If change impacts >3 downstream units, escalate"
```

### The Guardrail Spec

```markdown
## Guardrails: Dependency Management

AUTONOMOUS ACTIONS:
  - Update patch versions (x.x.PATCH)
  - Run test suite after update
  - Revert if tests fail

ESCALATE TO T3 (AI decides, human reviews):
  - Minor version updates (x.MINOR.x)
  - Any update that changes API surface

ESCALATE TO T2 (AI drafts, human decides):
  - Major version updates (MAJOR.x.x)
  - Updates with known breaking changes
  - Updates to security-critical dependencies

NEVER (stays at T1):
  - Framework changes (React → Vue)
  - Language version changes (Node 18 → 20)
```

---

## Audit Strategy

Human doesn't review individual outputs — audits the **system**:

```
WEEKLY AUDIT (15 min):
  - Review guardrail breach log (any escalations this week?)
  - Check key metrics (any anomalies in T4 activities?)
  - Sample 2-3 random outputs for quality

MONTHLY AUDIT (1 hour):
  - Review T4 activity list — should anything graduate to T5 or demote?
  - Check guardrail effectiveness — are they catching the right things?
  - Review escalation patterns — are guardrails too tight or too loose?

QUARTERLY AUDIT (half-day):
  - Full review of T4 boundaries
  - Evaluate new activities for T4 candidacy
  - Update guardrail specs based on accumulated evidence
```

---

## Graduation Criteria (T4 → T5)

```
├── Zero escalations for 90+ days
├── Audit samples show zero issues for 6+ months
├── Activity is COMMODITY (no competitive value, no alignment impact)
├── Full rollback capability exists (if anything goes wrong)
└── Multiple organizations do this identically (industry standard)
```

## Demotion Criteria (T4 → T3)

```
├── Guardrail breach with material impact
├── Audit reveals pattern of undetected issues
├── Domain changed (new regulations, new requirements)
├── Team lost context on what T4 activities are doing
└── Guardrail specs are outdated
```

---

## Who Does What

| Activity | Human | AI |
|---|---|---|
| Define guardrails | Authors guardrail spec | Proposes guardrails from patterns |
| Operate within guardrails | Not involved | Full autonomy |
| Detect guardrail breach | Reviews escalations | Monitors and escalates |
| Periodic audit | Conducts audit | Prepares audit report and samples |
| Update guardrails | Approves changes | Proposes adjustments from data |
| Graduation/demotion | Decides level changes | Proposes based on metrics |
