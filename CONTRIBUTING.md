# Contributing to Aesth

Guidelines for contributing to the aesth plugin.

## Getting Started

### Prerequisites

- Claude Code with plugin support
- Subcog MCP server configured and running
- Basic understanding of Claude Code plugin architecture

### Subcog Setup (Required)

Before developing, ensure Subcog MCP is properly configured:

1. Install Subcog MCP server:
   ```bash
   # Follow instructions at https://github.com/zircote/subcog
   ```

2. Configure Subcog in your Claude Code settings (`.mcp.json`):
   ```json
   {
     "mcpServers": {
       "subcog": {
         "command": "subcog",
         "args": ["serve"]
       }
     }
   }
   ```

3. Verify Subcog is running:
   ```
   /subcog:status
   ```

Without Subcog, aesth commands will fail as it has no file-based fallback.

### Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/zircote/aesth.git
   cd aesth
   ```

2. Symlink or copy to your Claude Code plugins directory

3. Restart Claude Code

4. Verify installation:
   ```
   /aesth:init
   ```

## Project Structure

```
aesth/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── .claude/
│   ├── commands/            # Slash commands
│   │   ├── init.md
│   │   ├── status.md
│   │   ├── validate.md
│   │   ├── capture.md
│   │   └── extract.md
│   ├── skills/
│   │   └── aesth/
│   │       ├── SKILL.md     # Main skill definition
│   │       └── references/
│   │           └── principles.md
│   └── hooks/               # Event hooks (disabled by default)
│       └── *.bak            # Preserved hook templates
├── README.md
├── CLAUDE.md                # Development guidelines
├── CONTRIBUTING.md
├── ARCHITECTURE.md
└── CHANGELOG.md
```

## Development Guidelines

### Commands

Commands live in `.claude/commands/` as markdown files with YAML frontmatter:

```markdown
---
name: aesth:example
description: Brief description of what this command does
---

# Command Title

Instructions for Claude to follow...
```

**Requirements:**
- Clear, actionable instructions
- Document all parameters
- Include example usage
- Handle error cases gracefully

### Skills

The main skill definition is in `.claude/skills/aesth/SKILL.md`. When modifying:

- Keep the skill focused on interface design for tools/apps
- Maintain Subcog as the exclusive memory backend
- Follow the "invisible assistant" communication style
- Always offer to save patterns after completing tasks

### Design Principles

All contributions should align with aesth's core principles:

1. **4px Grid**: All spacing multiples of 4
2. **Depth Consistency**: Choose one approach (borders/shadows) and commit
3. **Color for Meaning**: Gray for structure, color for communication
4. **Typography Hierarchy**: Clear distinction between headlines, body, data

## Subcog Memory Patterns

### Memory Structure

Aesth uses the `patterns` namespace with these memory types:

| Type | Tags | Domain |
|------|------|--------|
| Design Direction | `aesth`, `design-direction` | project |
| Design Tokens | `aesth`, `design-tokens` | project |
| Component Patterns | `aesth`, `component-pattern` | project |
| Design Decisions | `aesth`, `design-decision` | project |
| Craft Principles | `aesth`, `craft-principles` | user |

### Adding New Memory Types

1. Define clear tag conventions
2. Document in SKILL.md
3. Update relevant commands to query/capture the new type
4. Add validation rules if applicable

## Testing

### Manual Testing

1. Initialize on a fresh project:
   ```
   /aesth:init
   ```

2. Build a component and verify pattern suggestions

3. Test validation:
   ```
   /aesth:validate src/components/
   ```

4. Verify Subcog memory capture and recall

### Test Scenarios

- New project with no existing design system
- Project with existing Subcog memories
- Validation against established patterns
- Pattern extraction from existing code

## Pull Request Process

1. **Fork** the repository
2. **Create a branch** for your feature/fix
3. **Make changes** following the guidelines above
4. **Test** thoroughly with manual testing
5. **Update documentation** if adding features
6. **Submit PR** with clear description

### PR Requirements

- Clear description of changes
- Documentation updates for new features
- No breaking changes to existing commands
- Follows existing code style and patterns

## Reporting Issues

Use GitHub Issues for:
- Bug reports (include steps to reproduce)
- Feature requests
- Documentation improvements

## Questions?

Open a GitHub Discussion or reach out to the maintainers.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
