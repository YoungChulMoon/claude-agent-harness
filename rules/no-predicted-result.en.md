# No predicted results

Required for all agents and the orchestrator.

> **Do not draw conclusions or write code on top of a predicted or assumed result from someone
> else — the orchestrator, another agent, another system.**
> Get the actual result, verify it, and only then move to the next step.

## Why

- Predictions miss. Conclusions and code built on a wrong assumption are all rework.
- "The other team probably pushed it" / "that API will return this field" /
  "the orchestrator must have handed over X" — all of these are **things to verify**, not facts.
- This is the **cross-system extension** of the rule against inferring existing code and data.
  Not just existing code — *results produced or handed over by another party* are also
  off-limits for inference.

## Forbidden (proceeding on a prediction)

- Treating work another team or system *said* they did as fact, without measuring
- Assuming the schema or values of an API response you haven't received, and writing
  parsing/branching on top of it
- Treating a deliverable (SQL, file, package, commit) from the orchestrator or another agent
  as complete before receiving it
- Pre-writing downstream steps on "it will obviously work this way"

## Required (proceed after measurement)

1. **Actually receive** the result you depend on — look at the response, file, commit, log, DB row
2. **Verify** what you received — existence, schema, values, state
3. Conclude or continue coding only after verification passes
4. If the result doesn't exist yet → **leave it open, waiting or watched.** Do not proceed on assumption

Number 4 is the hard one in practice.
"Let's just assume it goes like this and keep moving" feels natural.
Leaving something open has to be established by rule as a better state than proceeding,
or it won't hold.

## Case (2026-06-26)

Another team reported that a search API had been pushed. It had not.
Had the deploy gone ahead on that assumption, it would have broken.

→ After the team acknowledged the missing push, the actual push and a measurement
(**11,059 indexed records**) were confirmed, and only then did the deploy proceed.

## Related

- The rule against inferring existing code and data — this one extends it across systems
- Cross-team handoff cycle — four-step handoff with receipt confirmation
- Post-hoc verification rule — label a narrative only after measured verification
