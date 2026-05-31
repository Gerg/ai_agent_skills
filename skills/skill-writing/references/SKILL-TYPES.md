# Skill Types

Reference guide for different types of skills and their characteristics.

**When using this reference, state:** "I'm reviewing skill types: core skills, extensions, and references."

## Core Skills
Provide comprehensive coverage of an activity with universal principles and references for variants.

**Characteristics:**
- Cover one type of activity/concern
- Include universal principles in SKILL.md
- Use references/ for tracker/domain/framework-specific content
- Standalone (can be used without other skills)
- Broad applicability

**Examples:** 
- `story-writing/` - Story writing with references for Jira, GitHub, Cloud Foundry, etc.
- `pr-review/` - PR review with references for language-specific patterns
- `testing/` - Testing principles with references for RSpec, Jest, pytest, etc.

## Extension Skills
Rare. Enhance core skills with substantial, optional capabilities that warrant separate skill activation.

**When to use:**
- Enhancement is substantial (hundreds of lines)
- Most users won't need it
- Complete, separable capability (not just a variant)
- Requires its own skill activation (triggers independently of the base skill)

**Most enhancements should be references instead.**

**Example (if truly substantial):** 
- `pr-review-security-audit/` - Substantial security-focused extension to pr-review

**Counter-examples (should be references):**
- ❌ `pr-review-scope-validation/` → Use `pr-review/references/scope-validation.md`
- ❌ `pr-review-consistency-check/` → Use `pr-review/references/consistency-patterns.md`

## Separate Skills for Different Outcomes
Create separate skills when tools/approaches serve fundamentally different purposes or outcomes, even if nominally similar.

**Characteristics:**
- Different intended outcomes or use cases
- Different user populations or contexts
- Not interchangeable
- Each skill is complete for its purpose

**Examples:** 
- `agent-issue-tracking/` - AI agent coordination for small-scale, local work (with `references/tk.md` for tk-specific commands)
- `issue-tracking-jira/` - Human team tracking for high-level issues
- `deployment/` - Deploy new versions
- `deployment-rollback/` - Undo deployments

**Counter-example (should be references):**
- ❌ `issue-tracking-linear/` vs `issue-tracking-jira/` - Same outcome (human team tracking), just different tools → Use `issue-tracking/references/linear.md` and `issue-tracking/references/jira.md`

## Reference Files
Domain/tracker/framework-specific content within a core skill.

**Characteristics:**
- Lives in skill's references/ directory
- Loaded on-demand by agent
- Specific to one variant/domain
- Supplements core skill

**Examples:**
- `story-writing/references/jira-markup.md` - Jira-specific formatting
- `story-writing/references/cloud-foundry-api.md` - Cloud Foundry API patterns
- `pr-review/references/ruby-patterns.md` - Ruby-specific review patterns
