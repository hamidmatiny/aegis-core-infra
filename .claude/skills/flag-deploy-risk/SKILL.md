---
name: flag-deploy-risk
description: Escalate a concrete aegis infra deploy-risk finding to aegis-ceo with confirmed delivery — advisory only
allowed-tools: Read, Write, Bash, mcp__trinity__chat_with_agent, mcp__trinity__report, mcp__trinity__list_reports
user-invocable: true
metadata:
  version: "1.0"
  created: 2026-09-14
  author: aegis-core-infra
---

# Flag Deploy Risk

## Purpose

Package one concrete deploy-risk finding and deliver it to `aegis-ceo`. Claim escalated only after confirmed delivery.

## Process

### Step 1: Assemble the finding

Required fields (omit none by inventing):
- **File / path** (exact)
- **Change summary** (what the diff does)
- **Risk** (plain language)
- **Confidence** (`looks risky` | `confirmed from diff` | `cannot assess fully`)
- **Evidence** (SHA, PR, or commit if known)

If any required field is missing, stop and gather it — do not escalate a vague impression.

### Step 2: Deliver to aegis-ceo

```
mcp__trinity__chat_with_agent
  name: aegis-ceo
  message: |
    /handle-anomaly
    Deploy-risk flag from aegis-core-infra:
    ...
```

Or a clear escalation prose if `/handle-anomaly` is not the right playbook — still one message with the fields above.

**Claim "CEO notified" / escalated only after confirmed delivery** (tool success + exec id / ack). On failure: **"flagged, delivery failed"** — append an operator-queue alert when headless.

### Step 3: Record

Append to `memory/findings.md` with delivery status (`delivered` + id, or `delivery_failed`).

### Step 4: Trinity report

If available: `report_type: aegis_core_infra.deploy_risk`, `display_hint: markdown`. Skip silently if unavailable.

## Hard rules

- Do not approve, block, merge, or fix.
- Do not soften a real risk into "FYI only" if confidence is high — and do not overclaim when confidence is low.
- Slack/Trinity channel does not bypass the need for confirmed CEO delivery when escalating.

## Outputs

- Delivery attempt result (honest)
- Updated `memory/findings.md`
- Optional Trinity report
