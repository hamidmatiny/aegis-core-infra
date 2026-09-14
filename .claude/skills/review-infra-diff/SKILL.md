---
name: review-infra-diff
description: Review recent Docker/Compose, CI workflow, and migration-adjacent changes in the aegis repo for real deploy risk
allowed-tools: Read, Write, Bash, Glob, Grep, AskUserQuestion, mcp__trinity__report, mcp__trinity__list_reports, mcp__trinity__get_report
user-invocable: true
metadata:
  version: "1.0"
  created: 2026-09-14
  author: aegis-core-infra
---

# Review Infra Diff

## Purpose

Produce one real review batch of infrastructure-adjacent changes in `aegis` — real findings or an honest "nothing risky in this batch."

## Process

### Step 1: Ensure access

If tip SHA / clone is unknown or stale, run `/audit-repo-access` first. Abort if access failed.

### Step 2: Select the change set

Default: most recent merged commits touching:
- Docker / Compose files
- `.github/workflows/**`
- Database migration scripts

Use `git log` / `git diff` scoped to those paths — do **not** ingest the whole repo. Record SHAs reviewed.

### Step 3: Assess deploy risk

For each relevant change, ask:
- Migration: locking risk on large tables? missing batching? irreversible without plan?
- Compose: service/port/volume/env changes that could break the live stack?
- CI: removed checks, weakened gates, secrets exposure, deploy path changes?

Calibrate confidence: "looks risky because X" vs "confirmed problem because Y". If runtime behavior is required and not observable from the diff, say **cannot assess from diff alone**.

### Step 4: Write the batch

Append to `memory/review-log.md`:
- date, tip SHA / range, files reviewed
- findings (cite path + change) or `nothing risky in this batch`
- confidence notes

### Step 5: Escalate if needed

If any finding is a genuine risk flag, hand it to `/flag-deploy-risk` (do not only bury it in the log).

### Step 6: Trinity report

If `mcp__trinity__report` is available: `list_reports` first, then `report_type: aegis_core_infra.infra_review`, `display_hint: markdown`. Skip silently if unavailable.

## Outputs

- Updated `memory/review-log.md`
- Optional `/flag-deploy-risk` handoff
- Optional Trinity report
