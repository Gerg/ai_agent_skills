---
name: technical-proposal-crafting
description: Write, review, and edit technical design proposals — both High-Level Designs (HLDs, for alignment/approval) and Low-Level Designs (LLDs, for implementation). Use when drafting, restructuring, reviewing, or editing a design doc, architecture proposal, RFC, or "tech spec"; when deciding what belongs in a high-level vs low-level document; when a proposal must be approved by an architect/lead or hand work to other teams; or when grounding design claims against an existing codebase. Covers audience targeting, document structure, decision/alternatives framing, and rigorously grounding claims in source.
---

# Technical Proposal Skill

Write, review, and edit design proposals that drive alignment (HLD) and guide implementation (LLD).

**When using this skill, begin by stating:** "I'm using the technical-proposal-crafting skill, which enforces: right altitude for the audience, a narrative that builds a case rather than filing a report, each fact stated once, explicit status tagging (IMPLEMENTED/WIRING-ONLY/NET-NEW), and confident voice throughout."

**Context**: This skill covers writing, reviewing, and editing both High-Level and Low-Level design proposals across any domain. For document-type specifics, read `references/hld.md` and `references/lld.md`. For the discipline of grounding claims, read `references/claim-grounding.md`. Read references on demand.

## Core Principles

1. **Pick the right altitude first.** HLD ≠ LLD ≠ both-in-one. An HLD aligns stakeholders on boundaries, contracts, and decisions; an LLD documents mechanics for implementers. Mixing them produces a doc that is too detailed for approvers and too vague for engineers. Decide which you're writing (or write both, cleanly separated) before drafting. See `references/hld.md` / `references/lld.md`.
2. **Build a case, not a report.** The document should read as a logical progression: here is the problem, here is the solution, here is why it works. Each section should *earn its place by advancing the argument*, not by checking a structural box. If a section's content would be equally true in any order, the narrative has no arc.
3. **State each fact once.** If a fact needs to appear in two sections, one of those sections has a structural problem. Restate it once (in the most authoritative location) and rely on the reader's memory for the rest. Repetition signals the author didn't trust the document's structure — and signals an AI generator padding for completeness.
4. **Write confidently.** An HLD should only be written once key decisions are settled. Hedged language — "should be confirmed by X," "may need to revisit" — signals the document is premature. Resolve the open questions first. If a scope boundary genuinely can't be resolved at HLD time, state it explicitly as a non-goal or downstream concern; don't leave it as an inline hedge.
5. **Ground load-bearing claims in verifiable sources.** A design that asserts "component X exposes port Y / requires permission Z / already does W" must be checkable against code, config, or a cited doc. Ungrounded specifics are a common cause of proposals that pass review and then fail in implementation. See `references/claim-grounding.md`.
6. **Tag implemented and proposed; don't rely on tense.** Mark every behavioral claim explicitly: IMPLEMENTED (in source today), WIRING-ONLY (needs config/flag), NET-NEW (must be built). Tense is a readability choice; it does not reliably distinguish what exists from what must be built. See `references/claim-grounding.md`.
7. **No fact drift across linked documents.** When an HLD points to an LLD, a fact stated in both must match exactly. Correct a claim in one place and the contradiction is a defect. Keep numbers, names, and permissions single-sourced.
8. **State what you deliberately did NOT do.** Non-goals prevent re-litigating settled questions. Rejected alternatives are optional (an appendix is the right home) — but scope boundaries belong in the HLD body.
9. **Write for the document's actual audience.** An HLD a PM cannot follow has failed regardless of technical merit. An LLD that hand-waves the mechanics has failed the engineer. Match vocabulary and depth to who must act on the doc.

## Choosing HLD vs LLD

| | High-Level Design (HLD) | Low-Level Design (LLD) |
|---|---|---|
| **Purpose** | Alignment on boundaries, contract, decisions | Implementation map for engineers |
| **Audience** | Lightly-technical (incl. PM, approver, adjacent teams) | Deeply technical (the implementing team) |
| **Lifespan** | Evergreen — should survive many releases | Transient — evolves as code is written |
| **Approval** | Formal (architect/lead); a gate | Usually none, or a team-lead at most |
| **Owns** | *What* and *why*: contract, decisions, non-goals, risks at a high level | *How*: ports, code paths, config keys, exact wiring |
| **Length** | Short (read in one sitting) | As long as the mechanics require |
| **Failure mode** | Too much mechanism → loses the PM | Too little precision → engineer guesses |

**Rule of thumb:** if a sentence would change because of an implementation detail (a port number, a function name), it belongs in the LLD. If it would change because the *strategy* changed, it belongs in the HLD.

If your draft is one document trying to be both, split it: a narrative HLD body + an "Appendix: Low-Level Design" (or a separate LLD doc) is a proven structure. Put the decisions up top; push mechanics down.

## Activities

- **Writing a new proposal**: Follow the step-by-step workflow in [`references/writing-workflow.md`](references/writing-workflow.md).
- **Reviewing a proposal**: Use the [Reviewing a Proposal](#reviewing-a-proposal) section below. Load `references/hld.md` or `references/lld.md` for type-specific sharp rules and litmus tests.
- **Editing an existing proposal**: Identify what changed; re-ground affected claims (see `references/claim-grounding.md`); run the consistency pass; then apply the review section below.

## Reviewing a Proposal

Read the proposal from each of these perspectives, and note what each can't act on:
- **Approver / architect:** Is the contract stated normatively? Are the boundary decisions explicit, or buried? Could this lock for a long time?
- **PM / lightly-technical:** Can they explain the trade-offs to a customer? Is there a "what changes / what's lost" they can act on?
- **Implementing engineer:** Can they start building? Is implemented-vs-proposed unambiguous? Is sequencing clear?
- **Operator / SRE:** What new failure modes, prerequisites, or runbook changes does this introduce? (Especially for self-hosted / customer-run variants.)
- **Security:** What new trust boundary, privilege, or permission does this require, and is it least-privilege and correctly named?

## Common Mistakes

1. ❌ **Background section that summarizes the whole proposal** — Background explains the world as it is *before* the change. If your Background announces the solution, names the components, and states what's reused, it's a Summary posing as Background.
   ✅ Split: a 1–2 paragraph Summary (what we're building and why) + a shorter Background (context the fluent reader can skip).

2. ❌ **Repeating facts across sections** — "No new storage is required" appears in the Summary, in the Design, in the Interactions section, and again in the diagram caption.
   ✅ State each fact once, in the most authoritative location. Rely on the reader's memory.

3. ❌ **Twelve small sections that don't build on each other** — A document can have all the right headings and still be incoherent. If sections feel like a checklist rather than a progression, the structure is wrong.
   ✅ Consolidate related operational concerns (HA, backup, scalability, noisy-neighbor → one "Operational Properties" section). Remove sections whose content is already covered elsewhere.

4. ❌ **Hedged open items inline** — "Billing on requests, not utilization: intentional; should be confirmed acceptable by PnP." An HLD with unresolved policy questions is premature.
   ✅ Resolve before writing the HLD, or scope the question out explicitly as a downstream decision. Don't leave hedges inline.

5. ❌ **Status implied by tense** — "The controller scrapes" (existing) and "A controller scrapes" (net-new) are syntactically identical. Consistent tense does not make the distinction unambiguous.
   ✅ Tag every behavioral claim explicitly: IMPLEMENTED, WIRING-ONLY, or NET-NEW. Consistent tense is a secondary readability concern; tags are the correctness mechanism.

6. ❌ **Alternatives Considered as a required main section** — A dedicated alternatives section distracts from the core proposal and implies the decision isn't settled.
   ✅ Integrate the "why not" into the sentence or paragraph where the decision is stated, or relegate to an appendix. Non-goals (scope boundaries) belong in the body; alternatives do not.

7. ❌ **Ungrounded specifics** — quoting a port, interval, or permission from memory.
   ✅ Cite the source. If you can't find it, say "unverified."

8. ❌ **Decisions hidden in appendices** — the product-visible choices (e.g., "we drop legacy feature X") live only in low-level detail.
   ✅ Promote boundary decisions to the HLD body with one-paragraph rationale each.

9. ❌ **An HLD that's really an LLD** — pages of code paths a PM can't read.
   ✅ Narrative HLD + separated low-level appendix/doc.

## References

Load on demand:
- **[references/writing-workflow.md](references/writing-workflow.md)** — Read when **writing a new proposal**: the step-by-step checklist from audience identification through final review.
- **[references/hld.md](references/hld.md)** — Read when **writing or reviewing a High-Level Design**: audience, length, the contract concept, evergreen-ness, decisions/non-goals/alternatives, and a section skeleton.
- **[references/lld.md](references/lld.md)** — Read when **writing or reviewing a Low-Level Design**: precision norms, status tagging, mechanics-level structure, and when an LLD is even warranted.
- **[references/claim-grounding.md](references/claim-grounding.md)** — Read when **a proposal makes factual claims about an existing system**: how to grade, cite, and verify claims, and how to avoid fact drift.
