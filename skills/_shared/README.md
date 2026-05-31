# Shared Skill Resources

This directory contains resources that are used by multiple skills. These files are symlinked into individual skill `references/` directories to maintain a single source of truth while allowing each skill to reference them naturally.

## Why Shared Resources?

Some principles and guidance apply universally across multiple skills. Rather than duplicating content or creating awkward cross-skill references, we use symlinks to share resources while maintaining:

1. **Single source of truth** - Content exists in one place
2. **Natural references** - Each skill references `references/filename.md` as if it were local
3. **Guaranteed consistency** - Impossible for content to diverge
4. **Clear shared status** - Files in `_shared/` are explicitly multi-skill resources

## Current Shared Resources

### code-quality.md

Universal code quality principles that apply both when **writing code** and when **reviewing code**.

**Used by:**
- `code-development/references/code-quality.md` → `../../_shared/code-quality.md`
- `pr-review/references/code-quality.md` → `../../_shared/code-quality.md`

**Content:**
- Comment quality (avoid comments unless absolutely necessary)
- Naming clarity (match existing codebase style)
- Redundancy detection (avoid multiple mechanisms)
- Test pressure (all logic must be driven by test requirements)
- Understanding the codebase (study patterns, flag bad code)

## Adding New Shared Resources

When you identify content that applies to multiple skills:

1. **Create the file in `_shared/`**
   ```bash
   touch skills/_shared/new-resource.md
   ```

2. **Create symlinks in each skill that needs it**
   ```bash
   cd skills/skill-name/references
   ln -s ../../_shared/new-resource.md new-resource.md
   ```

3. **Update this README** to document the new shared resource

4. **Reference it naturally** in each skill's SKILL.md:
   ```markdown
   See [references/new-resource.md](references/new-resource.md) for details.
   ```

## Git and Symlinks

Git handles symlinks well:
- Symlinks are stored as symlinks in the repository
- Cloning the repo preserves symlinks on Unix/Mac
- On Windows, git may check out symlinks as text files containing the link path (depending on git config)

## Multi-Source Considerations

The `_shared/` pattern works well for single skill repositories but presents challenges when managing skills from multiple sources:

### Dependency Discovery

There's currently no standard way to know which skills depend on `_shared/` without examining their `references/` directories for symlinks. Skills that reference shared resources will have broken references if `_shared/` is missing.

**Current skills with `_shared/` dependencies:**
- `code-development` → `_shared/code-quality.md`
- `pr-review` → `_shared/code-quality.md`

### Namespace Collisions

If multiple skill repositories use `_shared/`, their shared resources could conflict when merged into a single discovery location. For example:
- Repository A has `_shared/testing-patterns.md`  
- Repository B also has `_shared/testing-patterns.md`
- When both are installed to `~/.cursor/skills/`, the second overwrites the first

### Recommendations

**For skill installation:**
- Always symlink entire skill directories when possible (avoids missing dependencies)
- Use separate discovery locations for different skill sources
- Use tools like [stow](https://www.gnu.org/software/stow/) for namespace management

**For future evolution:**
The Agent Skills standard may need to address:
- Shared dependency declaration in skill metadata
- Namespace management for multi-source installations
- Standardized shared resource patterns across different skill repositories

## Alternatives Considered

**Cross-skill references** (e.g., `../pr-review/references/code-quality.md`):
- ❌ Creates coupling between skills
- ❌ Unclear which is the "source of truth"
- ❌ Awkward navigation

**Duplication**:
- ❌ Content can diverge over time
- ❌ Updates must be applied multiple places
- ❌ Maintenance burden

**Symlinks** (current approach):
- ✅ Single source of truth
- ✅ Natural references
- ✅ Clear shared status
- ✅ Easy maintenance
- ⚠️ Requires coordination across skill sources
