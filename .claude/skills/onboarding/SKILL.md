---
name: onboarding
description: Track your setup progress — shows what's done, what's next, and walks you through each step
allowed-tools: Read, Write, Edit, Bash, AskUserQuestion
user-invocable: true
metadata:
  version: "1.0"
  created: 2026-09-14
  author: aegis-core-infra
---

# Onboarding

Track and continue your setup progress. This skill reads `onboarding.json`, shows your current status, and walks you through the next incomplete step.

## Process

### Step 1: Load State

Read `onboarding.json` from the agent root directory. If it doesn't exist, inform the user that onboarding is complete or the file was removed.

### Step 2: Show Progress

Display a checklist grouped by phase. Mark the current phase with an arrow. Use checkboxes:

```
## AEGIS Core Infra — Setup Progress

### Phase 1: Local Setup  ← current
- [ ] Configure environment variables (.env)
- [ ] Run /audit-repo-access
- [ ] Run /review-infra-diff once (trial)
- [ ] Install recommended plugins

### Phase 2: Trinity Deployment
- [ ] Deploy to Trinity (mid-cost OmniRoute)
- [ ] Run a skill remotely

### Phase 3: Schedules
- [ ] Confirm recommended schedules
- [ ] Verify first scheduled execution

**Progress: 0/8 complete**
```

### Step 3: Guide Next Step

Identify the first incomplete step in the current phase. Based on which step it is, provide specific guidance:

**For `env_configured`:**
- Check if `.env` exists. If not, guide: `cp .env.example .env` then fill in values.
- `AEGIS_REPO_URL` is required. `GITHUB_TOKEN` only if HTTPS clone against a private repo.
- After user confirms, mark done.

**For `first_skill_run`:**
- Tell the user to run `/audit-repo-access`.
- Mark done only after a successful tip SHA / access ok (or an honest failed-access record — still mark done so the gap is visible; do not invent success).

**For `trial_review`:**
- Tell the user to run `/review-infra-diff` once as the Initial Scope trial pass.
- Mark done after `memory/review-log.md` has one real batch (findings or nothing-risky).

**For `plugins_installed`:**
- Run:
  ```
  /plugin install agent-dev@abilityai
  /plugin install trinity@abilityai
  /plugin install utilities@abilityai
  ```
- After all attempted, mark done.

**For `onboarded` (Trinity phase):**
- Guide `/trinity:onboard`.
- Remind: tier is **mid-cost OmniRoute**, not Claude Pro subscription — escalate to `aegis-infra` if deploy lands on subscription.
- After completion, mark done and advance phase.

**For `first_remote_run`:**
- Tell user to run `mcp__trinity__chat_with_agent` with `/audit-repo-access` or `/review-infra-diff`.
- After completion, mark done and advance phase.

**For `schedules_configured`:**
- Confirm `template.yaml` schedules match CLAUDE.md. Daily review stays `enabled: false` until Hamid sets cadence.
- Mark done after confirmation.

**For `first_scheduled_run`:**
- Only after a schedule is armed — verify one execution completed. Otherwise leave incomplete with a note.

### Step 4: Persist

Write updated `onboarding.json` after each completed step. When all steps in a phase are done, set `phase` to the next phase name (`local` → `trinity` → `schedules` → `complete`).

## Outputs

- Updated `onboarding.json` with progress
- Step-by-step guidance for the current task
- Phase transition messages at milestones
