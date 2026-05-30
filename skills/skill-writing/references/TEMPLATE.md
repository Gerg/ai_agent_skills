# Skill Template

Copy-paste template for creating new skills. Save as `SKILL.md` in a directory named after your skill (matching the `name` field).

**When using this reference, state:** "I'm using the skill template to create a new skill."

```markdown
---
name: skill-name
description: Brief description of what this skill does and when to use it. Include triggering conditions and use cases.
---

# [Skill Name] Skill

Brief description of what this skill covers and when to use it.

**When using this skill, begin by stating:** "I'm using the [skill-name] skill, which [brief description of what it enforces or provides]."

<!-- For core skills with references, add: -->
**Context**: This skill covers [activity] across all [trackers/domains/frameworks].
For [tracker/domain/framework]-specific guidance, see the references/ directory.

<!-- For extension skills, replace the Context line with: -->
<!-- **Prerequisites**: This skill extends [Base Skill](../base-skill/SKILL.md).
Use this extension when [specific scenario]. -->

## Core Principles

1. **Principle 1** - Explanation
2. **Principle 2** - Explanation
3. **Principle 3** - Explanation

## [Main Section 1]

### [Subsection]

Content with examples.

✅ Good: [example]
❌ Bad: [example]

### [Another Subsection]

More content with examples.

## [Main Section 2]

### [Pattern or Type 1]

Description and example.

```
Example code or content
```

### [Pattern or Type 2]

Description and example.

### Example: [Scenario Name]

[Setup/context for the example]

[Example code or content]

[Explanation of what the example demonstrates]

## Common Mistakes

1. ❌ **Mistake** - Why it's wrong
   ✅ **Correct** - How to do it right

2. ❌ **Another mistake** - Why it's wrong
   ✅ **Correct** - How to do it right

## Quick Reference

[Optional: Summary or cheat sheet]

<!-- Choose ONE of the following two sections based on skill type: -->

## References

<!-- For core skills with references — list each reference file and when to read it: -->
- **[Reference Name](references/reference-name.md)** - When/why to read it

<!-- OR for extension/standalone skills that compose with other skills: -->
## Related Skills

Use this skill with:
- **[Skill A](../skill-a/SKILL.md)** - When/why
- **[Skill B](../skill-b/SKILL.md)** - When/why
```

---

## Reference File Template

For files in `references/`, use this header:

```markdown
# [Reference Title]

**When using this reference, state:** "I'm applying [reference-name] guidance: [key principles]."

[Content]
```
