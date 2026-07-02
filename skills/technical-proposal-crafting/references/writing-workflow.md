# Writing Workflow

**When using this reference, state:** "I'm following the technical proposal writing workflow: audience + altitude → decisions first → grounding → draft → consistency pass → review."

Copy this checklist and track progress:

```
Proposal Progress:
- [ ] Step 1: Identify audience + altitude (HLD, LLD, or both)
- [ ] Step 2: Capture the contract/decisions/non-goals before prose
- [ ] Step 3: Ground every load-bearing claim against source (file:line / URL)
- [ ] Step 4: Draft in the document's template, decisions-first
- [ ] Step 5: Tag every behavioral claim (IMPLEMENTED / WIRING-ONLY / NET-NEW); check linked-doc consistency
- [ ] Step 6: Review through each audience lens
```

**Step 1 — Audience + altitude.** Name the approver and the teams who'll build against this. Decide HLD/LLD/both. Read `hld.md` or `lld.md`.

**Step 2 — Decisions first.** Before writing prose, list: the contract/interface this exposes; the 2–5 real decisions and their chosen option; the non-goals; the top risks. Everything else is support.

**Step 3 — Ground claims.** For each specific factual claim (a port, a permission, an "already does X"), find and cite the source. **Do not proceed to polished prose with ungrounded specifics.** See `claim-grounding.md`.

**Step 4 — Draft.** Use the team's template if one exists; otherwise the structures in the reference files. Background is "skippable for the fluent." Put alternatives + rationale in an appendix or inline as the decision demands.

**Step 5 — Consistency pass.** Tag every behavioral claim IMPLEMENTED / WIRING-ONLY / NET-NEW. Diff every fact that appears in both a linked HLD and LLD; they must match.

**Step 6 — Review.** Read the draft through each audience lens (see Reviewing a Proposal in SKILL.md). Fix what each can't act on.
