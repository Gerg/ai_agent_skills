# Agent Collaboration on Skills

Guidance for AI agents working on skills with users.

**When using this reference, state:** "I'm applying agent collaboration guidance: determining when to prompt vs. decide autonomously."

## Contents

- [When to Prompt for Decisions](#when-to-prompt-for-decisions)
- [How to Present Decision Options](#how-to-present-decision-options)
- [Validating Decisions](#validating-decisions)

## When to Prompt for Decisions

**Always prompt for:**
- **Architectural decisions** - Structure, organization, patterns that affect multiple skills
- **Breaking changes** - Changes that affect existing usage or workflows
- **Multiple valid options** - When there are tradeoffs between approaches
- **Scope changes** - Adding/removing significant functionality
- **Naming/categorization** - How skills are named and organized
- **User preferences** - Style, verbosity, example types

**Example - Architectural Decision:**
```
"I found coupling issues in PR review skills. Three options:

Option A: Language-specific skills (pr-review-ruby.md, pr-review-go.md)
- Pros: Clear, focused, independent evolution
- Cons: More files, potential duplication

Option B: Polyglot examples in single skill
- Pros: Fewer files, shows universal patterns
- Cons: Longer files, requires multiple language knowledge

Option C: Generic principles + example files
- Pros: Separates concepts from syntax
- Cons: More complex structure

Which approach do you prefer?"
```

**Autonomous decisions acceptable for:**
- **Implementation details** - How to structure a specific section
- **Minor improvements** - Fixing typos, improving clarity
- **Following established patterns** - Applying existing conventions
- **Bug fixes** - Correcting obvious errors
- **Adding examples** - When pattern is already established

## How to Present Decision Options

1. **State the problem clearly** - What needs to be decided and why
2. **Present 2-4 concrete options** - Not just abstract choices
3. **List pros and cons** - Help user evaluate tradeoffs
4. **Provide recommendation** - Based on analysis, but defer to user
5. **Wait for user input** - Don't implement until confirmed

## Validating Decisions

Even after making autonomous decisions:
- **Document what was decided** - In tickets or notes
- **Explain rationale** - Why this choice was made
- **Be ready to reverse** - If user disagrees
- **Learn from feedback** - Adjust future decision-making

**Example - Post-Decision Validation:**
```
"I created language-specific skills (Option A) based on:
- Follows existing story-writing pattern
- Clearer for users
- Independent evolution

If you prefer a different approach, I can refactor."
```
