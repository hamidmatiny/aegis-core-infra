# Architecture — AEGIS Core Infra (current state)

Descriptive. Tracks what exists today. When a TARGET item ships, move it here.

## Role

Personal Trinity agent — sixth hire, first Engineering specialist. Reports to `aegis-ceo`. Reviews infrastructure-adjacent diffs in `github.com/hamidmatiny/aegis` and flags deploy risk. Advisory only.

## Skills (shipped)

| Skill | Role |
|-------|------|
| `/audit-repo-access` | Prove read-only clone/fetch works; record tip SHA |
| `/review-infra-diff` | Batch-review Docker/Compose, workflows, migrations |
| `/flag-deploy-risk` | Deliver a concrete finding to `aegis-ceo` |
| `/onboarding` | Setup checklist |
| `/update-dashboard` | Refresh `dashboard.yaml` from memory |
| `/reconcile-docs` | Doc/skill coherence (report-only on schedule) |

## Data

| Path | Purpose |
|------|---------|
| `memory/review-log.md` | Append-only review batches |
| `memory/findings.md` | Append-only escalations + delivery status |
| `onboarding.json` | Setup progress |
| `dashboard.yaml` | Live snapshot for Trinity |

## Schedules

Declared in `template.yaml` — all `enabled: false` until Hamid arms them. Recommended: daily `/review-infra-diff` (07:00 UTC), `/update-dashboard` every 6h, weekly `/reconcile-docs`.

## Tier

Mid-cost OmniRoute (API-key routing), per `aegis-infra` proposal. Not Claude Pro subscription.

## Not in scope (today)

- Blocking merges / CI gates
- Writing to `aegis` or production
- Auto-remediation of flagged issues
