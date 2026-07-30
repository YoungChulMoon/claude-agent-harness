# Case study: cutting 22 agents down to 17

> Five agents retired in the 2026-07-16 consolidation. Definition files are preserved, not deleted
> (for restoration and history).
>
> **This document was reconstructed after the fact, on 2026-07-27.** At the time of the
> consolidation the retirement reason was not written into each file — measured: only 1 of 5 had one —
> so opening the archive later told you nothing about why anything was retired. This fills that gap.

There are plenty of write-ups about growing your agent count. Fewer about cutting it.
This is the latter, and also **a record of failing to write things down in time and having to
reconstruct them.**

## 1. Reconstructing the numbers (measured)

`22 → 17` counts **agents in operation**, not files. Counting files alone doesn't add up.

| | `agents/*.md` (excl. template) | outside `agents/` | total |
|---|---|---|---|
| Before consolidation | **21** | +1 | **22** |
| After consolidation | **16** | +1 | **17** |
| Current | **16** | +1 | **17** |

- The `+1` outside `agents/` is a standalone-session agent. Its definition lives in a `CLAUDE.md`
  on a separate path, so there's no file in `agents/`, but it is listed in the delegation table
  and therefore counted.
- Verified: `git ls-tree --name-only {commit}^ .claude/agents/` = 21 / `{commit}` = 16 /
  current `ls` = 16 (zero file changes in `agents/` since; diff is empty)
- The five were moved as an `R100` rename, not a delete (content 100% identical) →
  restorable exactly as they were.

> When you report a number, define what it counts first.
> "22→17" on its own won't match a file count later, and then nobody can verify it.

## 2. The five — where they went

| # | Retired | Disposition | Absorbed by | Reason |
|---|---|---|---|---|
| 1 | Content-writing agent | **Replaced by skills** | 2 skills | Repetitive work with a fixed procedure and no contextual judgment. Applied the "tools/skills first, agents last" principle. Skills cost **zero tokens, are 100% reproducible, and don't forget between sessions** |
| 2 | Security-check agent | **Merged** | Code-audit agent | Code audit and security check overlapped in the same read-only domain. The audit side absorbed external-attack detection and auth checks |
| 3 | Cron-watch agent | **Merged** | Health-check agent | Watching cron execution is part of server health checking. Behavior rules moved with it |
| 4 | Planning agent | **Merged** | Execution agent | There was no real benefit to keeping planning and execution separate |
| 5 | External-chat agent | **Dormant** (not retired) | — | **Zero traffic for 70 days** on that feature. Process stopped, moved to dormant. Restore when the service resumes |

**The shared judgment behind the three merges (#2, #3, #4)**

> When two agents overlap in the same domain with the same permissions (read-only, or the same
> edit scope), **the cost of keeping them separate — orchestrator overhead, delegation latency —
> exceeds the benefit.**

## 3. Retiring is not deleting

The important part is that dispositions split four ways.

| Disposition | Meaning | Restoration |
|---|---|---|
| Demoted to skill | Should never have been an agent | **Do not restore** |
| Merged | Role absorbed elsewhere | Must be split back out of the absorber |
| Dormant | Domain is alive, traffic is zero | Restore as-is |
| Deleted | — | (never actually used) |

## 4. Restoration procedure

1. `git mv agents_archived/{name}.md agents/`
2. Restore its row in the delegation table and the agent definition list
3. **Remove the duplicated role from the absorbing agent** — skip this and two agents do the same job
4. Follow the creation protocol — owner approval required

★ **Anything demoted to a skill is not a restoration candidate.**
The skill is the correct path. Bringing it back violates "tools/skills first" and reintroduces
the duplication.

## 5. What this case leaves behind

1. **Write the reason into the file at the moment you retire it.** Otherwise you reconstruct it
   three months later. That is exactly what happened here.
2. **Define the number before reporting it.** State what it counts.
3. **Retire by rename.** Deleting removes the path back.
4. **Write down what must not be restored.** If you don't, someone eventually brings it back.
