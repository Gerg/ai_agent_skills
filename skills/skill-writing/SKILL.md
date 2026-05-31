---
name: skill-writing
description: Meta-skill for creating effective skill documents using progressive disclosure. Use when creating new skills, updating existing skills, reviewing skill quality and structure, organizing skills with references, or when users request help with skill design, organization, or best practices.
license: Apache-2.0 (see LICENSE file)
---

# Skill Writing Skill

A meta-skill for creating effective skill documents using progressive disclosure.

**When using this skill, begin by stating:** "I'm using the skill-writing skill to create/review skills following progressive disclosure principles."

**Context**: This skill covers skill authoring for all skill types (core, extension, reference files). Read references on demand — see the [References](#references) section at the bottom for when to load each one.

## What is a Skill?

A **skill** is a reusable knowledge document that provides structured guidance on how to accomplish a specific type of task. Skills are designed to be:

- **Focused** - Cover one concern or outcome
- **Extensible** - Support variants through references and optional extensions
- **Prescriptive** - Provide clear, actionable guidance
- **Generic** - Avoid project-specific details
- **Reusable** - Apply across multiple contexts
- **Concise** - Respect the shared context window

## Skill Design Principles

### 1. Concise is Key

**The context window is a shared resource.** Skills compete for context with system prompts, conversation history, other skills, and the user's actual request.

**Default assumption: Agents are already capable.** Only add context the agent doesn't already have. Challenge each piece of information:
- "Does the agent really need this explanation?"
- "Does this paragraph justify its token cost?"

**Prefer concise examples over verbose explanations.**

**Target: Keep SKILL.md under 500 lines.** When approaching this limit, split content into reference files.

### 2. Sharp and Opinionated Over Generic

**Skills must change agent behavior.** If an agent would do the same thing without the skill, the skill has no value.

**Two questions to ask:**
1. **Effectiveness**: "Would an agent behave differently with this skill?" (If no → skill is too generic)
2. **Efficiency**: "Does the agent already understand this concept?" (If yes → explanation is redundant)

**Examples of sharp, opinionated guidance:**
- ✅ "Avoid code comments unless absolutely necessary; Code & tests should be self-documenting"
- ✅ "When starting a session, always affirm that you are conforming to the instructions"
- ✅ "All logic must be driven by test requirements (test pressure)"

**Examples of generic, useless guidance:**
- ❌ "Write clear code" (too vague, agents already do this by default)
- ❌ "Consider adding comments when appropriate" (softened, no clear directive)
- ❌ "Follow best practices" (meaningless without specifics)

**Sharpness checklist:**
- Use imperatives, not suggestions ("Do X" not "Consider X" or "You might want to X")
- Be specific about what to do and what not to do
- Include concrete examples and anti-patterns
- Don't soften language during editing ("avoid X" stays "avoid X", not "consider avoiding X")
- When extracting from existing guidance (AGENTS.md, team docs), preserve the original wording
- User feedback on skills comes from hard-won experience - don't dull the edge when incorporating it

**The goal:** Skills should result in code or behavior that is stylistically divergent from what agents would produce by default.

### 3. Set Appropriate Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

**High freedom (text-based instructions)**: Use when multiple approaches are valid, decisions depend on context, or heuristics guide the approach.

**Medium freedom (pseudocode or structured guidance)**: Use when a preferred pattern exists, some variation is acceptable, or configuration affects behavior.

**Low freedom (specific scripts, templates, few parameters)**: Use when operations are fragile and error-prone, consistency is critical, or a specific sequence must be followed.

Think of the agent as exploring a path: a narrow bridge with cliffs needs specific guardrails (low freedom), while an open field allows many routes (high freedom).

### 4. Single Responsibility
Each skill should focus on one concern or outcome. Use progressive disclosure (references/) for variants.

✅ Good:
- `story-writing/` - Story writing with references for different trackers/domains
- `pr-review/` - PR review with references for different validation types
- `deployment/` - Deployment with references for different platforms

❌ Bad:
- `development/` - Too broad, covers everything
- `jira-api-story-writing-cloud-foundry-cli/` - Multiple concerns in one skill

### 5. When to Create Separate Skills vs Use References

- **Separate skills**: For orthogonal concerns — different activities or outcomes (e.g., `deployment/` vs `deployment-rollback/`)
- **References**: For variants within a concern — different tools, domains, or frameworks (e.g., `story-writing/references/jira-markup.md`)
- **Extensions**: Rare — only when the enhancement is substantial, optional, and requires its own skill activation

See [SKILL-TYPES.md](references/SKILL-TYPES.md) for the full taxonomy with examples and counter-examples.

### 6. Clear Prerequisites and Context
State what other skills are required and guide users to relevant references. Use a `**Context**:` statement for core skills and a `**Prerequisites**:` statement for extensions — see [TEMPLATE.md](references/TEMPLATE.md) for the exact format.

### 7. Avoid Duplication
Use references for domain-specific content and shared resources for multi-skill content.

When the same content applies to multiple skills, use symlinks to a shared source rather than copying — see [ORGANIZATION.md](references/ORGANIZATION.md) for the full pattern.

❌ Bad: copying the same content into two skill files — maintenance burden, guaranteed divergence.

### 8. Generic Over Specific
Keep skills applicable across contexts. Extract project-specific details.

✅ Good:
```markdown
Use consistent placeholder data throughout examples.
```

❌ Bad:
```markdown
Use potato-themed examples (e.g., "spud-managers", "potato-user").
```

### 9. Show, Don't Just Tell
Provide concrete examples, not just abstract principles.

✅ Good:
```markdown
### Example Structure
```
Given a user has sufficient permissions
When POST to /resources with valid data
Then the request succeeds with status code 201
```
```

❌ Bad:
```markdown
Include preconditions, actions, and expected outcomes.
```

## Skill Structure

### YAML Frontmatter

Skills should begin with YAML frontmatter containing metadata:

```markdown
---
name: skill-name
description: Brief description of what this skill does and when to use it. Used by agents to determine relevance.
license: MIT (optional)
compatibility: Requires Python 3.8+ (optional)
disable-model-invocation: false (optional, default false)
---
```

**Required Fields:**
- `name` - Skill identifier. Lowercase letters, numbers, and hyphens only. Must match the parent directory name.
- `description` - **Critical for skill triggering.** Describes both WHAT the skill does AND WHEN to use it. Include all triggering conditions here (not in the body, since the body loads only after triggering). Be comprehensive about use cases and contexts.

**Optional Fields:**
- `license` - License name or reference to a bundled license file
- `compatibility` - Environment requirements (system packages, network access, etc.)
- `metadata` - Arbitrary key-value mapping for additional metadata
- `disable-model-invocation` - When `true`, the skill is only included when explicitly invoked. When `false` (default), agents automatically apply it based on context.

### Essential Sections

Use [TEMPLATE.md](references/TEMPLATE.md) as the authoritative template for skill structure. The one section requiring explicit guidance:

#### Skill Affirmation

**Required for all skills and all reference files.** This makes skill usage visible — it confirms the agent loaded the skill and sets expectations for the user.

```markdown
**When using this skill, begin by stating:** "I'm using the [skill-name] skill, which [brief description of what it enforces or provides]."
```

Write the affirmation to name the specific behaviors the skill enforces, not just what topic it covers:

✅ `"...which enforces opinionated best practices: minimal comments, full test coverage, test pressure for all logic"`
❌ `"...to help with code development"`

### Optional Sections

- **Common Mistakes** - What to avoid, with ❌/✅ examples (see [ANTI-PATTERNS.md](references/ANTI-PATTERNS.md) for skill-writing anti-patterns)
- **Quick Reference** - Cheat sheet or summary
- **Workflows** - Multi-step processes with checklists (see [Content Patterns](references/CONTENT-PATTERNS.md))
- **Templates** - Output format templates (see [Content Patterns](references/CONTENT-PATTERNS.md))
- **Advanced Topics** - Deep dives for power users
- **Troubleshooting** - Common issues and solutions

### What NOT to Include

A skill should only contain essential information for agents to do the job. **Do NOT create auxiliary documentation files:**

❌ **Avoid:**
- README.md (separate from SKILL.md)
- INSTALLATION_GUIDE.md
- QUICK_REFERENCE.md
- CHANGELOG.md
- CONTRIBUTING.md
- User-facing documentation
- Setup and testing procedures
- Process documentation about creating the skill
- Prior art or provenance (this belongs in the repository's README.md)

✅ **Include only:**
- SKILL.md (the skill itself)
- scripts/ (executable code if needed)
- references/ (documentation loaded as needed)
- assets/ (files used in output)

The skill should contain only what an agent needs to execute the task, not auxiliary context about the skill's development or maintenance.

## Content Guidelines

### Progressive Disclosure

**Progressive disclosure is the primary pattern for managing skill complexity.** Skills use a three-level loading system:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<500 lines recommended)
3. **Reference files** - As needed by agent (loaded on demand)

**When to use references:**
- SKILL.md approaching 500 lines
- Multiple trackers/tools supported (Jira, GitHub, Linear)
- Multiple domains/platforms (Cloud Foundry, Kubernetes, AWS)
- Multiple frameworks/variants (RSpec, Jest, pytest)
- Detailed reference material (markup guides, API patterns)
- Conditional features (advanced topics, troubleshooting)

For splitting patterns (tracker-specific, domain-specific, framework variants, conditional details), see [CONTENT-PATTERNS.md](references/CONTENT-PATTERNS.md).

**Guidelines:**
- Keep SKILL.md focused on universal principles and navigation
- Put tracker/domain/framework-specific content in references/
- Keep references one level deep (link directly from SKILL.md)
- For files >100 lines, include table of contents at top
- Reference files clearly from SKILL.md with context about **when** to read them
- **Reference files should be command references, not tutorials** — assume agents understand concepts from SKILL.md; references provide syntax, options, and quick examples only

### Content Patterns

See [CONTENT-PATTERNS.md](references/CONTENT-PATTERNS.md) for workflows, feedback loops, template patterns, and reference splitting patterns.

### Include Anti-Patterns
Show what *not* to do, with explanations.

```markdown
### Common Mistakes

1. ❌ Mixing multiple concerns in one skill
   - Makes skills hard to reuse
   - Bloats the skill with unrelated content
   - Reduces clarity and focus
   
   ✅ Instead: Create focused, single-concern skills with references for variants
```

### Link Between Skills and References

For linking templates (extensions, references, related skills), see [TEMPLATE.md](references/TEMPLATE.md).

## Skill Organization

For file structure, naming conventions, and shared resources, see [ORGANIZATION.md](references/ORGANIZATION.md).

## References

Load these on demand — only when the task requires it:

- **[TEMPLATE.md](references/TEMPLATE.md)** — Read when **creating or structuring a new skill**
- **[SKILL-TYPES.md](references/SKILL-TYPES.md)** — Read when **deciding whether to create a separate skill, add a reference, or create an extension**
- **[CREATION-PROCESS.md](references/CREATION-PROCESS.md)** — Read when **creating a new skill from scratch** or **running a quality review on an existing one**
- **[CONTENT-PATTERNS.md](references/CONTENT-PATTERNS.md)** — Read when **implementing workflows, feedback loops, or output templates**, or when **deciding how to split a large skill into references**
- **[ANTI-PATTERNS.md](references/ANTI-PATTERNS.md)** — Read when **reviewing a skill for common mistakes**
- **[AGENT-COLLABORATION.md](references/AGENT-COLLABORATION.md)** — Read when **deciding whether to prompt the user for a decision or proceed autonomously**
- **[ORGANIZATION.md](references/ORGANIZATION.md)** — Read when **setting up the file structure, naming, or shared resources for a skill**
