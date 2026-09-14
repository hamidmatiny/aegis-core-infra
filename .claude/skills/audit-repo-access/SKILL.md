---
name: audit-repo-access
description: Confirm read-only clone access to the aegis product repo actually works before claiming any review
allowed-tools: Read, Write, Bash, Glob, Grep, AskUserQuestion, mcp__trinity__report, mcp__trinity__list_reports
user-invocable: true
metadata:
  version: "1.0"
  created: 2026-09-14
  author: aegis-core-infra
---

# Audit Repo Access

## Purpose

Prove that read-only access to `github.com/hamidmatiny/aegis` works from this agent's environment — before any review claim.

## Process

### Step 1: Locate config

Read `.env` / `.env.example` for `AEGIS_REPO_URL` and optional `GITHUB_TOKEN`. If `AEGIS_REPO_URL` is missing, stop and ask Hamid — do not invent a path.

### Step 2: Clone or fetch (read-only)

Prefer a shallow clone or fetch into a workspace path such as `~/repos/aegis` or `/tmp/aegis-readonly` (never a write push remote).

```bash
# Example — adjust to the configured URL; never push
git ls-remote "$AEGIS_REPO_URL" HEAD
```

If clone already exists, `git fetch` and record the tip SHA.

### Step 3: Spot-check infra paths exist

Confirm these paths (or honest absences) on the tip commit:
- `docker-compose*.yml` / `compose*.yaml`
- `.github/workflows/`
- migration directories (e.g. `**/migrations/**`, Alembic, or project-specific paths)

### Step 4: Report

State: URL used (no secrets), tip SHA, which infra paths exist, and **access: ok | failed** with the exact error if failed.

If `mcp__trinity__report` is available: `report_type: aegis_core_infra.repo_access`, `display_hint: kpi` or `markdown`. Skip silently if unavailable.

## Outputs

- Honest access verdict with tip SHA
- Optional Trinity report
- Never a fabricated "repo looks fine" without a successful fetch
