# High-Level Design (HLD)

**When using this reference, state:** "I'm applying HLD guidance: align a lightly-technical audience on contract, decisions, and non-goals — evergreen, short, narrative arc."

An HLD's job is **alignment**, not instruction. It defines the boundary of a box a team has autonomy within. It is the document an approver signs and adjacent teams build against.

## What a good HLD is

- **Lightly-technical.** A product manager for the area should be able to read it and discuss the trade-offs. If a PM can't follow it, it's too low-level.
- **Short.** Readable in one sitting. If it's long, you usually have *too much* detail, not too little — push mechanics to an LLD or appendix.
- **A narrative.** The sections should build a logical progression: problem → solution → why it works → what changes. A document whose sections could be reordered without loss has no arc and will feel assembled rather than written.
- **A contract.** It defines the interface/behavior the component exposes to other teams or users, stable enough that others can build against a mock of it. State this contract **normatively and in one place** ("the component emits X with names/tags identical to Y"), not scattered as implications.
- **A high-level internal design.** Functional blocks and technology choices — plus the critical considerations teams forget: **RBAC/permissions, HA & scalability, failure modes, data durability/backup, encryption/trust boundaries.** Name them even if the answer is "N/A, because…".
- **Unified.** Each fact appears exactly once, in the section where it carries the most weight. If a fact appears in three sections, two of those sections have structural problems. Repetition tells the reader the author didn't trust the document's structure.
- **Evergreen.** Aim for a design that could stay valid across many releases. Decisions are integrated into the section where they apply — not collected in a "Design Decisions" log at the end. The reader cares about the outcome, not the deliberation history.

## Section skeleton

A reliable HLD skeleton (adapt to any house template). Sections marked **optional** are often valuable but not always required — omit if genuinely not applicable, not to save space.

1. **Header** — title, author, approver, date, status, related tickets/docs.
2. **Summary** — what and why, in 1–2 paragraphs. The proposal, not the context. A reader should be able to stop here and know what this builds.
3. **Background** — only enough context to make the design legible; mark it skippable for the fluent. Strictly context — not solution, not rationale. If it explains what you're building rather than why the problem exists, it belongs in the Summary.
4. **The contract / integration interface** — the normative statement of what the component exposes: API shape, label conventions, naming rules, metrics endpoint. State this *before* the internal design so readers understand the commitment before the mechanism.
5. **The design** — functional blocks, data/control flow, key design choices with inline rationale. Diagrams earn their space. Integrate rationale where the decision is made; don't defer it to an appendix.
6. **Critical considerations** — RBAC, HA/scalability, failure modes, encryption, operational impact. Related concerns can be consolidated into one section (e.g., HA + backup + scalability → "Operational Properties") — prefer fewer, fuller sections over many small ones.
7. **What changes for users / operators** — the product-visible deltas, including anything *lost*. A few bullets or a short table. *(optional but strongly encouraged)*
8. **Non-goals** — what is explicitly out of scope, with a brief why. *(optional, include when scope is genuinely contestable)*
9. **Alternatives considered** — rejected options with one-sentence why-not. *(appendix or omit; do not make this a main section that competes with the proposal itself)*

## Sharp rules

- **Summary ≠ Background.** Background explains the world as it is *before* the change. If a paragraph announces the solution, names the new components, or states what's reused, it belongs in the Summary. A Background that announces the proposal trains readers to skim Background sections in future docs.
- **Write confidently.** An HLD with unresolved policy questions ("should be confirmed by X") is premature. Resolve open questions before writing the HLD — or scope them out explicitly as downstream decisions. Inline hedges undermine the document's authority and mislead the approver about how settled the design is.
- **Integrate decisions, don't collect them.** Rationale for a choice belongs in the section where the choice is explained, not in a "Design Decisions" section at the end. An evergreen HLD reads as a confident statement of how the system works; a decision log reads like meeting notes. The approver cares about the outcome, not the deliberation trail.
- **Consolidate related operational sections.** HA, backup/restore, scalability, and noisy-neighbor are all "how does this behave in production?" questions. Splitting them into four sections of three bullets each fragments the picture. Merge into one "Operational Properties" section.
- **Promote product-visible decisions out of any appendix.** "We don't support legacy feature X on the new runtime" is an HLD-body decision with rationale, not a low-level footnote.
- **One "what changes" section beats scattered caveats.** If users/operators see new behavior, collect it in one place the PM can act on — even if the list is short.
- **Don't overload a term.** If two components share a generic name (e.g. two "collectors," two "agents"), disambiguate on first use; ambiguity quietly corrupts every later section.
- **State trust boundaries explicitly.** "Authenticates as identity I, requires exactly permission P (least-privilege)" is reassuring and prevents over-broad grants downstream.

## Reviewer's litmus tests

Beyond completeness checks, test the document's reader experience:

1. **The 30-second test.** Can a reader answer "what is this proposing, and why?" without reading past page 1? If not, the Summary or opening paragraph is wrong.
2. **The reorder test.** Pick any two non-adjacent sections and swap them. Does anything break? If sections are independent, the document has no narrative arc.
3. **The repetition test.** Pick any key fact (e.g., "no new storage is required"). Search for it. If it appears in more than one section, one of those occurrences is redundant.
4. **The hedge test.** Search for "should be confirmed," "may need to," "pending." If any appear, the HLD is premature or the scope boundary isn't stated clearly enough.
5. **The PM test.** Could a PM for this area read this and explain the trade-offs to a customer? If they'd need a separate briefing, the document is pitched too low.

## Contrast lesson (HLD derived from a richer draft)

When an HLD is summarized from a longer working draft, corrections made in the draft can fail to propagate, and present-tense descriptions of not-yet-built components slip in. Before circulating: re-diff the HLD against the source of truth. A polished HLD that asserts a capability exists — when it's still net-new — is the most common way an HLD misleads its approver about cost.
