# Copilot Instructions

You are working in the aesth plugin repository for Claude Code.

## Project Overview

This is a Claude Code plugin for craft-focused design system management powered by Subcog memory. Build interfaces with elegance, consistency, and semantic memory.

## Key Components

- **Commands**: 5 commands (capture, extract, init, status, validate)
- **Hooks**: Design validation hooks
- **Subcog Integration**: Leverages Subcog memory for design patterns

## Plugin Structure

```
.claude-plugin/plugin.json  # Plugin manifest
.claude/commands/           # 5 design system commands
hooks/                      # Design validation hooks
```

## Development Guidelines

1. Follow Claude Code plugin standards
2. Keep changes focused and reviewable
3. Update CHANGELOG.md for user-facing changes
4. Test commands locally with `claude --plugin-dir .`

## Testing

```bash
claude --plugin-dir .
```

Then test:
- `/aesth:init` command
- `/aesth:capture` command
- `/aesth:validate` command
- `/aesth:status` command
- `/aesth:extract` command
