# Agent Enforcement

Global rules and hooks that ensure specific skills are always active in each tool, regardless of auto-discovery. Each enforcement file instructs the agent to use a named skill — the skill itself remains the single source of truth.

---

## Installing enforcement on your machine

### Cursor

```bash
mkdir -p ~/.cursor/rules
ln -s /path/to/repo/enforcement/cursor/rules/agent-collaboration.mdc \
      ~/.cursor/rules/agent-collaboration.mdc
```

Restart Cursor after adding a rule. Verify it appears in **Cursor Settings → Rules**.

### Claude Code

**1. Symlink the hook script:**

```bash
ln -s /path/to/repo/enforcement/claude-code/hooks/agent-collaboration.sh \
      ~/.claude/hooks/agent-collaboration.sh
```

**2. Register the hook in `~/.claude/settings.json`:**

Add the following to the `hooks` block (see `claude-code/settings-fragment.json` for the full JSON structure):

```json
"SessionStart": [
  {
    "hooks": [
      { "type": "command", "command": "~/.claude/hooks/agent-collaboration.sh" }
    ]
  }
]
```

Claude Code settings [merge across layers](https://code.claude.com/docs/en/settings) — hooks from user, project, and project-local settings all fire. If a skill only needs to apply within a specific project, you can put the `SessionStart` entry in `.claude/settings.json` at the project root instead.

### Codex

```bash
mkdir -p ~/.codex
ln -s /path/to/repo/enforcement/codex/AGENTS.md ~/.codex/AGENTS.md
```

Verify with:
```bash
codex --ask-for-approval never "Summarize the current instructions."
```

---

## Adding enforcement for a new skill

To enforce a skill, add one file per tool:

**Cursor** — add `cursor/rules/<skill-name>.mdc`:
```
---
alwaysApply: true
---
Use the <skill-name> skill.
```

**Claude Code** — add `claude-code/hooks/<skill-name>.sh` (copy `agent-collaboration.sh` as a template):
```bash
echo "Use the <skill-name> skill."
```

Then register it in `~/.claude/settings.json` following the same pattern as the existing hook.

**Codex** — append to `codex/AGENTS.md`:
```
Use the <skill-name> skill.
```
