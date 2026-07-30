# Agent lifecycle rules

When to create an agent, and when to retire one.

## Deciding to create

Most of the time the answer is "don't."

| Situation | Decision |
|---|---|
| Falls within an existing agent's role | Delegate to the existing one |
| One-off investigation or task | Orchestrator does it directly |
| **Repetitive work — fixed procedure, no judgment needed** | **Create a tool/skill, not an agent** — priority gate |
| Recurring pattern + new domain needing contextual judgment | **Create an agent** |
| Domain where 2+ agents already overlap | **Create an agent** |

Row three is the one that catches most often. If the procedure is fixed and no judgment is
required, it's a tool, not an agent.
Without this gate the agent count only goes up.

## Creation protocol

1. Orchestrator notices the need
2. Proposes it to the owner **in one line**
3. On approval, create immediately from `agent-template.en.md`
4. Update the delegation table
5. Fold in feedback after the first task

## Deciding to retire

| Situation | Disposition |
|---|---|
| Turns out to be a fixed procedure with no judgment | **Demote to skill/tool** — zero tokens, 100% reproducible, no session amnesia |
| Overlaps another agent in the same domain with the same permissions | **Merge** — the cost of staying separate exceeds the benefit |
| Zero traffic in its domain for a long stretch | **Dormant** — not retired. Restore when it resumes |

Definition files are **moved to `agents_archived/`, never deleted.**
Moving by rename preserves the content exactly, so it can be restored as-is.

> **Write the retirement reason into the file at the moment you retire it.**
> If you don't, opening the archive later tells you nothing about why it was retired.
> This was missed once, and had to be reconstructed after the fact.
> → `case-studies/agent-consolidation-22-to-17.en.md`

## Managing by type

| Type | Location | Lifespan |
|---|---|---|
| Permanent | `agents/` | Indefinite |
| Temporary | `agents/temp/` | Archived when the issue is resolved |
| Experimental | `agents/temp/` | Review after 2 weeks → promote or delete |

## Naming temporary agents

`agents/temp/{role}_{YYYYMMDD}.md`
e.g. `agents/temp/log-anomaly-scanner_20260317.md`

The date is in the filename **so that neglect becomes visible.**
If it's still in `temp/` after two weeks, it's up for review.

## Self-fixup rule

> An analysis-type agent may directly fix a minor problem it finds during its own work.
> The point is to remove the overhead of routing every trivial fix back through the
> orchestrator to an edit-capable agent.

This was promoted to a rule later, after the inefficiency showed up in operation.
It was not designed up front.

### Scope

Limited to analysis, audit, and monitoring agents.
Does not apply to agents whose main job is editing.

### Conditions (all must hold — AND)

1. **One file only** (no multi-file edits)
2. **Under five lines changed** (no large refactors)
3. **Not a top-tier file** (shared libraries, classes, and other high-tier paths excluded)
4. **Below the impact threshold** (score from the in-house verification tool)

If any one fails, escalate.

### Obligations

- **Back up** — `{backup_path}/{filename}.bak_$(date +%Y%m%d%H%M%S)`
- **Write a work log** — can be short, but must exist
- **Update its own report** — reflect it in recent work history
- **Run the verification script** — the static checker for that stack

### Forbidden

- Top-tier files, DB changes, anything affecting external systems → **always escalate**
- If a self-fixup turns out to affect another area → report it
- **If it's unclear whether the conditions hold → request delegation**

That last item is the important one. The default has to be conservative.
The moment "handle it myself when ambiguous" becomes the tendency, the whole rule stops working.
