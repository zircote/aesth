# CLAUDE.md

Development guidelines for the Aesth plugin.

## Overview

Aesth is a Claude Code plugin for craft-focused design system management, powered by Subcog memory. It stores design tokens, patterns, and decisions in Subcog rather than file-based configuration.

## Architecture

```
.claude-plugin/
  plugin.json          # Plugin manifest
.claude/
  commands/            # Slash commands (/aesth:init, /aesth:validate, etc.)
  skills/
    aesth/
      SKILL.md         # Main skill definition
      references/      # Quick-reference documentation
```

## Key Concepts

### Memory-First Design

Aesth uses Subcog as the exclusive source of truth. No file-based `system.md` fallback.

- **Namespace**: `patterns`
- **Domain**: `project` for project-specific, `user` for universal craft principles
- **Tags**: Always include `aesth` plus memory-type tag

### Memory Types

| Type | Tags | Purpose |
|------|------|---------|
| Design Direction | `aesth`, `design-direction` | Personality, foundation, depth |
| Design Tokens | `aesth`, `design-tokens` | Spacing, colors, typography |
| Component Patterns | `aesth`, `component-pattern` | Button, Card, Input specs |
| Design Decisions | `aesth`, `design-decision` | Choices with rationale |
| Craft Principles | `aesth`, `craft-principles` | Universal rules (user domain) |

## Development

### Adding Commands

Commands go in `.claude/commands/`. Use this frontmatter:

```yaml
---
name: aesth:command-name
description: Brief description
allowed-tools: mcp__plugin_subcog_subcog__subcog_recall, mcp__plugin_subcog_subcog__subcog_capture
---
```

### Modifying the Skill

The main skill lives at `.claude/skills/aesth/SKILL.md`. Key sections:
- Subcog Integration (session start, memory structure)
- Design Principles (spacing, depth, typography)
- Workflow (communication style, suggest+ask pattern)

### Testing

Test commands by invoking them in Claude Code:
```
/aesth:init
/aesth:status
/aesth:validate src/components
```

## Subcog Memory Protocol

When working on this plugin:

1. **Session start**: Call `subcog_init` to load context
2. **Before changes**: `subcog_recall` for existing patterns/decisions
3. **After changes**: Capture significant decisions via `subcog_capture`

## Code Style

- Commands should be concise and focused
- Use markdown formatting consistently
- Include clear usage examples
- Follow the "invisible" communication pattern (no mode announcements)
