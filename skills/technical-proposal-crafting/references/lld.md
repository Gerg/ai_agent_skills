# Low-Level Design (LLD)

**When using this reference, state:** "I'm applying LLD guidance: precise mechanics for implementers, status-tagged, transient, and consistent with its parent HLD."

An LLD's job is to be a **map for the engineers doing the work** — ports, code paths, config keys, exact wiring. It is written for a deeply technical audience and is often indecipherable to non-engineers (that's fine). It is **transient**: it evolves as code is written. Over-investing in it produces a waterfall artifact that's stale on arrival.

## When to write one at all

Write an LLD only when the team needs up-front design to proceed — high complexity, unfamiliar work, or the lead can't be continuously available. A blanket "always write an LLD" requirement tends toward waterfall. If the team can build it from the HLD plus conversation, skip the LLD.

## What good LLD content looks like

- **Exact and citable.** Name the file, function, port, env var, config key, default value. Pair specifics with a `file:line` or config path so a reader can jump straight to source. This precision is the entire value of an LLD.
- **Status-tagged.** Every behavioral claim is one of: **IMPLEMENTED** (exists in source), **WIRING-ONLY** (exists but needs config/overlay), **NET-NEW** (must be built). Without tags, engineers can't tell what they can rely on vs. must write. This is the single highest-leverage habit in an LLD.
- **Mechanism, not motivation.** The *why* lives in the HLD. The LLD says *how*. A one-line back-reference to the HLD decision is enough.
- **Honest about the unverified.** If a claim depends on a release/config you can't see, say "unverified — depends on X," don't assert a number.

## Structure

LLDs are naturally organized by subsystem/section rather than narrative. A workable pattern:

- A short preamble: who it's for, and "nothing below changes the HLD's conclusions."
- One section per subsystem, each with: the relevant code paths/config, exact values, and a status tag.
- Risk/edge-case notes attached to the subsystem they concern, each with a concrete mitigation and (ideally) an acceptance test.
- Tables for enumerable facts (ports, job placements, behavior→code-path mappings) — they're scannable and easy to keep correct.

## Sharp rules

- **Tag implemented-vs-proposed in every section.** The worst LLD defect is describing a to-be-built receiver/container in the present tense, especially when the parent HLD says it doesn't exist yet — that's a self-contradiction that wastes the implementer's time.
- **A wrong specific is worse than a missing one.** An over-broad or mis-named permission, a wrong port, or a non-existent config key will be copied verbatim into real artifacts (a ClusterRole, a manifest). Verify each before stating it.
- **A "mitigation" that assumes a knob that doesn't exist is a code change, not a config default.** If you recommend "set option O," confirm O is actually a parameter; if it's hardcoded, the mitigation is "make O configurable, then set it" — say so.
- **Keep the parent HLD and the LLD in lockstep.** Any fact in both must match exactly. Decisions live in the HLD; if a decision only appears in the LLD, surface it to the HLD.
- **Don't restate the whole system.** The LLD covers what's intricate or new. Stock/unchanged behavior gets a one-line "unchanged from baseline" pointer.

## Contrast lesson (LLD precision is the point)

A strong LLD reads like it was written with the source open — because it should be. The failure mode isn't usually vagueness; it's **confidently-stated specifics that are subtly wrong** (a default that's actually different, a permission that doesn't exist, a component said to be deployed that isn't). Because the LLD is the artifact engineers trust most for exactness, these errors are higher-impact here than anywhere else. Re-verify specifics against source as a final pass (see `claim-grounding.md`).
