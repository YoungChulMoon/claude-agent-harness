# claude-agent-harness

Agent operating rules that came out of one developer running 30 projects in parallel with Claude Code.
Four years of running them for real. Still in use every day.

These are not rules for **building** agents. They are rules for keeping several of them from
**breaking things while running at the same time**.

> 한국어 원본: [README.md](README.md)

---

## Three things

**Never silently work around a block**

When an agent hits a constraint, it does not push through. It reports.
Even when the block looks like a misjudgment, the agent does not route around it —
it writes up the evidence and waits for confirmation.

And when a block reveals a flaw in the rules themselves,
**fixing that flaw is part of the response.**

**Never record a FAIL as a PASS**

Even when an item is closed by instruction, the measured state goes in as-is,
along with the conditions for reopening it.
Reports use before/after measurements, and remaining risk comes before results.

**22 → 17**

I grew the agent count to 22, then cut it back to 17.
Growing was easy. Deciding what to retire was not.
What got retired and why: [case study](case-studies/agent-consolidation-22-to-17.en.md).

---

## What's in here

| File | Contents |
|------|----------|
| [`agent-template.en.md`](agent-template.en.md) | Skeleton shared by every agent |
| [`rules/no-predicted-result.en.md`](rules/no-predicted-result.en.md) | Stops agents from inventing results they never verified |
| [`rules/agent-lifecycle.en.md`](rules/agent-lifecycle.en.md) | When to create, when to retire + self-fixup thresholds |
| [`case-studies/`](case-studies/) | The actual consolidation record |

Korean-only for now: `rules/orchestrator-craft.md` (9 orchestration patterns),
`rules/backup-rollback.md`, `examples/health-checker.md`.

---

## A few core patterns

**SSOT reference — no copy-pasting source text**
Agent docs hold only the summary needed to make a decision. Details and numbers point to the original.
Context cost drops, and when the original changes you don't edit it in a dozen places.

**Separating judgment from execution**
Verification and investigation fan out in parallel as read-only subagents.
File and DB execution runs serially through the center.
If several agents edit a shared file at once, it will break.

**Cascade blocking**
An agent cannot decide alone based only on another agent's output.
Any context it received is recorded with source and timestamp.

**Gate flag**
If a flag file is absent from a given path, the entire agent is a no-op. Default off.
Creating the file starts it. Deleting the file stops it.

**Autonomy defined by quantitative thresholds**
Instead of an allowed/forbidden binary, the self-edit scope is an AND condition:
one file, under five lines, not a top-tier file, impact score below threshold.
When it's ambiguous, escalating is the default.

---

## Using it

Nothing to install. These are markdown files.

```bash
git clone https://github.com/YoungChulMoon/claude-agent-harness
```

Put `agent-template.en.md` under `.claude/agents/` and fill in what you need.
Pick whichever rule docs fit your project.

---

## Assumptions

- Not tied to a specific model version. Preserved as procedure.
- Not the one right answer — just what worked in this environment for four years.
- May not fit your team size or project type.
- Domain-specific details (service names, server paths, accounts) are replaced with placeholders.

## License

MIT
