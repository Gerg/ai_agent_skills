# Skill Organization

File structure, naming, and shared resource conventions.

**When using this reference, state:** "I'm applying skill organization conventions: file structure, naming, and shared resources."

## File Structure

Each skill is a directory containing a `SKILL.md` file:

```
skill-name/
├── SKILL.md           # Required: metadata + instructions
├── scripts/           # Optional: executable scripts
│   └── deploy.sh
├── references/        # Optional: documentation loaded on demand
│   └── REFERENCE.md
└── assets/            # Optional: templates, configs, static files
    └── template.json
```

**Requirements:**
- Directory name must match the `name` field in frontmatter
- `SKILL.md` is the only required file

## Shared Resources

When content applies to multiple skills (e.g., code quality principles used when both writing and reviewing code), use symlinks to a shared source:

```
skills/
├── _shared/
│   └── code-quality.md               # Single source of truth
├── code-development/
│   └── references/
│       └── code-quality.md -> ../../_shared/code-quality.md
└── pr-review/
    └── references/
        └── code-quality.md -> ../../_shared/code-quality.md
```

**Benefits:** single source of truth, guaranteed consistency, can't diverge.

**When to use:**
- Same content applies to multiple skills and must stay in sync
- Universal principles that span activities (e.g., code quality for writing and reviewing)

**When NOT to use:**
- Content specific to one skill
- Content likely to evolve differently per skill
- Small duplications that don't justify the symlink complexity

**Creating shared resources:**
1. Create file in `_shared/` directory
2. Create symlinks in each skill's `references/` directory
3. Document in `_shared/README.md`

## Naming Conventions

Name skills by the concern, activity, or outcome they address. Domains, tools, and variants go in `references/`.

**Pattern:** `[concern]/`

**Examples:**
- `story-writing/` — story writing (references for trackers/domains)
- `pr-review/` — PR review (references for languages/patterns)
- `deployment/` — deployment (references for platforms/tools)

**Rules:**
- Lowercase with hyphens, not underscores
- Descriptive but concise
- Directory name must match `name` field in frontmatter
- Avoid redundant suffixes (`skill-writing` not `skill-writing-skill`)

When two outcomes are genuinely different (e.g., `agent-issue-tracking/` for AI coordination vs `issue-tracking-jira/` for human team tracking), more specific names are appropriate. Most cases should use a single skill with references for variants.

## README as Navigation Guide

The repository `README.md` should:
- List all available skills with brief descriptions
- Note which skills have `references/` directories
- Show how skills compose (e.g., pr-review + agent-issue-tracking)
- Provide quick start guides
