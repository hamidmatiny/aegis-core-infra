---
name: reconcile-docs
description: Check that CLAUDE.md, README, ARCHITECTURE/TARGET-ARCHITECTURE, skills, and subagents are mutually consistent — reports drift and applies approved fixes. Run after shipping a capability or on a schedule.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, AskUserQuestion
user-invocable: true
metadata:
  version: "1.0"
  created: 2026-09-14
  author: aegis-core-infra
  changelog:
    - "1.0: Initial version — dependency-graph-driven coherence check"
---

# Reconcile Docs

> ℹ️ **First, set expectations:** before anything else, print one short line with this skill's version and its most recent change — e.g. `reconcile-docs v1.0 — recent: Initial version`. Then proceed.

Keep this agent's documentation honest. Reads the **Artifact Dependency Graph** in `CLAUDE.md`, checks artifacts against sources, reports drift, and interactively applies approved fixes to descriptive targets only.

**Direction rule:** source wins. Fix `README.md` / `ARCHITECTURE.md` when they drift. Flag (do not auto-edit) `CLAUDE.md` / `TARGET-ARCHITECTURE.md`.

## Process

### Step 1: Load the graph

Parse `## Artifact Dependency Graph` in `CLAUDE.md`.

### Step 2: Gather reality

```bash
find .claude/skills -name SKILL.md 2>/dev/null
ls .claude/agents/*.md 2>/dev/null
ls README.md ARCHITECTURE.md TARGET-ARCHITECTURE.md template.yaml 2>/dev/null
ls memory/*.md 2>/dev/null
```

### Step 3: Check coherence

Record CONSISTENT / DRIFT / MISSING for:
1. CLAUDE.md ↔ skills (Core Capabilities + Request Dispatch)
2. README ↔ reality
3. ARCHITECTURE ↔ reality (including memory files)
4. TARGET-ARCHITECTURE ↔ ARCHITECTURE (shipped items moved)
5. Schedules ↔ `template.yaml`
6. Guidelines ↔ skills (no skill claims write/deploy/block authority)

### Step 4: Report

Produce a drift table. Scheduled runs are report-only.

### Step 5: Apply (interactive only)

Skip on schedule. Interactively propose edits to descriptive targets; never silently rewrite CLAUDE.md.

## Outputs

- Drift report
- Optional approved doc fixes
