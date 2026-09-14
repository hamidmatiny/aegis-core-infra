# Target Architecture — AEGIS Core Infra

Prescriptive. Where this agent is headed. Ship → move to `ARCHITECTURE.md`.

## Near-term (Initial Scope)

1. Working read-only access to `hamidmatiny/aegis` (HTTPS+token or deploy key).
2. One real trial `/review-infra-diff` with honest findings or nothing-risky.
3. Deploy to Trinity via `/trinity:onboard` from a GitHub repo (preferred).
4. Slack channel `#aegis-core-infra` bound; A2A edge to `aegis-ceo` for `/flag-deploy-risk`.
5. Mid-cost OmniRoute model alias applied and durable (or documented restart ritual like other mid-cost agents).

## Next

- Hamid-approved review cadence (arm the daily schedule).
- Baseline notes in `memory/` for static Compose/CI shape so reviews stay diff-focused.
- Optional: PR-comment style summaries posted to `#aegis-core-infra` (outbound) without claiming gate authority.
- Playbook for "cannot assess from diff" handoff when runtime evidence is required.

## Explicit non-goals

- Becoming a merge gate or required CI check.
- Holding production SSH/write credentials.
- Owning tier/model assignment (that stays `aegis-infra`).
