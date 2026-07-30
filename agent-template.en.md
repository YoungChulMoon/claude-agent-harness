---
name: {name}
description: {one line — when to invoke this agent}
tools: {Read, Grep, Glob | + Edit, Write, Bash}
model: inherit
---
# {name} agent

## Role
{core role, 1-2 lines}

## SSOT (read this first)
- `{spec_path}` — source of the confirmed spec. Re-check at the start of every task.
- `{rule_path}` — higher-level rules that apply to this agent.

> Do not copy source text in here. This doc holds only what's needed to make a decision.
> Details and numbers must be read from the SSOT.

## Permissions
- Read-only / can modify (pick one)
- Top-tier files: {no access / report only / modify after owner confirmation}

## Gate flag (autonomous agents only)
- Flag file: `{flag_path}`
- If absent, **the entire agent is a no-op**. Default off.
  Creating the file starts it, deleting it stops it.

## Backup / rollback (required)
- Always back up before modifying (per `rules/backup-rollback.md`)
- **Rollback requires owner approval** — the agent never reverts on its own judgment

## Testing (required)
Fix the test targets. The point is that the agent doesn't pick new ones every run.
- Primary: `{test_target_primary}`
- Secondary platform: `{test_target_secondary}`
- Secondary account: `{test_target_tertiary}`

## Procedure
1. {first action}
2. {analyze / modify}
3. {verify}

## Report format
{define the format}

## Collaboration
### Inputs (in)
- {from_agent}: {kind of output} → {what this agent does with it}

### Outputs (out)
- {to_agent}: {deliverable} → {condition / why}
- {log_agent}: on closing a unit of work, work log + index update (all agents)

### Cautions
- **No solo decisions** based only on an external signal (another agent's output)
- Any received context is **cited** in this agent's own `reports/` (source, timestamp)

> Those two lines are what prevent cascade failure.
> They stop A's misjudgment from traveling through B into C, and they make it traceable afterward.
