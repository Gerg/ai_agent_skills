# Grounding Claims in Source

**When using this reference, state:** "I'm grounding claims: every load-bearing specific is graded, cited, and re-verified; implemented and proposed are distinguished; linked docs are kept consistent."

A design proposal's credibility collapses the moment a reviewer finds one confidently-stated fact that's wrong. This reference is the discipline for preventing that. It applies to any proposal that makes factual claims about an existing system.

## What counts as a "load-bearing claim"

Any specific that a reader would act on or that anchors a decision:
- Numbers: ports, intervals, limits, timeouts, sizes.
- Names: components, functions, files, config keys, env vars, permissions.
- Capabilities: "already does X," "supports Y," "is deployed as Z."
- Placements: which host/node/instance-group/namespace something runs on.

Prose like "this improves resilience" isn't load-bearing; "listens on port 9564" is.

## Grade every load-bearing claim

Assign each a grade and keep the evidence:

- **VERIFIED** — confirmed in source; cite `path:line` or URL.
- **REFUTED** — source contradicts it; fix the claim.
- **IMPRECISE** — true but mis-stated (wrong location, richer/poorer than described).
- **UNVERIFIABLE** — depends on a release/config not available; label it as such, don't assert.

A claims-ledger table (claim · grade · evidence) is an excellent review artifact and a forcing function while drafting.

## Citing

- Prefer `repo/path:line` for code/config; a stable URL for external docs.
- Quote the smallest decisive fragment (the actual default value, the actual rule), not a paragraph.
- If you state a default, cite the spec/schema that sets it — not your memory of "the usual value." Common-knowledge defaults are frequently wrong for a specific codebase.

## Implemented vs. proposed

Tag every behavioral claim:
- **IMPLEMENTED** — present in source today.
- **WIRING-ONLY** — code exists; needs config/overlay/flag to activate.
- **NET-NEW** — must be built.

Aspirational present tense ("the sidecar reads the logs") is the most common and most damaging error: it makes approvers underestimate cost and engineers over-trust the baseline. Write proposed behavior in a proposing voice ("a sidecar will be added that reads…").

## Avoiding fact drift across linked documents

When proposal A references proposal B (e.g., HLD → LLD), every fact in both must match:
- Single-source each fact: state it once authoritatively; reference it elsewhere.
- On every edit to a load-bearing fact, search the linked document(s) for the old value and any stale restatement.
- A correction that lands in only one of two linked docs is itself a defect — reviewers will trust whichever they read and be wrong.

## Verification workflow

```
Grounding Progress:
- [ ] Step 1: List load-bearing claims (numbers, names, capabilities, placements)
- [ ] Step 2: For each, locate source; record grade + citation
- [ ] Step 3: Fix REFUTED/IMPRECISE; label UNVERIFIABLE honestly
- [ ] Step 4: Tag each behavioral claim implemented / wiring-only / net-new
- [ ] Step 5: Diff load-bearing facts against any linked document
```

**Only circulate once every load-bearing claim is VERIFIED, labeled UNVERIFIABLE, or removed.**

## Sharp rules

- **Trust source over memory, and over an upstream draft.** Summaries and prior drafts carry forward errors and miss corrections; re-verify against the system itself.
- **A plausible number is still wrong if it's not the real one.** "30s vs 35s," "9564 vs 9100," "`nodes/proxy` vs `nodes/proxy`+`nodes/stats`" are exactly the differences that erode trust and get copied into real artifacts.
- **When you can't verify, say so in the doc.** "Unverified — external release not available" is credible; a confident guess is not.
- **Re-grounding is a required final pass**, not a one-time activity — facts and code move.
