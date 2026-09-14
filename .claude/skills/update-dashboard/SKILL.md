---
name: update-dashboard
description: Refresh dashboard.yaml with current review and finding metrics from memory/
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, mcp__trinity__report
user-invocable: true
metadata:
  version: "1.0"
  created: 2026-09-14
  author: aegis-core-infra
---

# Update Dashboard

Refresh `dashboard.yaml` with current metrics from this agent's memory files.

## Process

### Step 1: Gather Metrics

Read:
- `memory/review-log.md` — batch count, last tip SHA, recent entries
- `memory/findings.md` — open vs delivered findings
- Recent git activity: `git log --oneline -10` (this agent repo, not aegis)

### Step 2: Update Dashboard

Update `dashboard.yaml`:
- `updated` timestamp to now
- "aegis repo access" from last `/audit-repo-access` note (green ok / red failed / gray unaudited)
- "Batches Logged", "Open Findings", "Last Tip SHA"
- "Recent reviews & flags" list from the last few memory entries
- "Own Tier" stays mid-cost unless you have verified drift

### Step 3: Publish KPI snapshot (Trinity)

If `mcp__trinity__report` is available:
- `report_type`: `aegis_core_infra.kpi_snapshot`
- `display_hint`: `kpi`
- Skip silently if unavailable

### Step 4: Confirm

Report what changed (old → new) for the headline widgets.

## Outputs

- Updated `dashboard.yaml`
- Optional Trinity KPI report
