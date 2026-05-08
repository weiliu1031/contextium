# Codex Standalone Configuration

Contextium supports Codex CLI as a primary agent with an `AGENTS.md` adapted from the core context router architecture.

## Optional Skill

Install the bundled `$contextium` skill to make session closeout a direct command:

```bash
mkdir -p ~/.codex/skills/contextium
cp agent-configs/codex/skills/contextium/SKILL.md ~/.codex/skills/contextium/SKILL.md
```

Then run:

```text
$contextium
```

Use `$contextium --no-push` to archive locally without pushing, or `$contextium --new-commit` to bypass the daily amend
behavior for a one-off separate commit.
