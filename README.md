# Aesth - Craft-Focused Design System Plugin

[![Version](https://img.shields.io/badge/version-0.1.0-blue.svg)](https://github.com/zircote/aesth/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Plugin](https://img.shields.io/badge/Claude_Code-Plugin-purple.svg)](https://github.com/anthropics/claude-code)
[![Requires Subcog](https://img.shields.io/badge/Requires-Subcog-orange.svg)](https://github.com/zircote/subcog)

Claude Code plugin for design system management powered by Subcog memory.

## Overview

Aesth (pronounced like "aesthetic") helps you build interfaces with craft and consistency by:

- **Storing design systems in Subcog** — Semantic, searchable memory that persists across sessions
- **Validating code against design tokens** — Catch spacing, depth, and color violations automatically
- **Learning from decisions across projects** — Universal craft principles inform all work
- **Providing intelligent direction** — Context-aware suggestions for new projects

## Installation

### From Marketplace (Recommended)

```bash
# Add the zircote marketplace (if not already added)
claude /plugin marketplace add zircote/marketplace

# Install aesth
claude /plugin install aesth@zircote
```

### From GitHub Directly

```bash
claude /plugin install zircote/aesth
```

### Manual Installation

1. Clone or symlink this directory to your Claude Code plugins location
2. Restart Claude Code

### Prerequisites

- Claude Code with plugin support
- Subcog MCP server configured and running

### Verify Installation

```
/aesth:init
```

## Commands

| Command | Description |
|---------|-------------|
| `/aesth:init` | Initialize skill and load design system from Subcog |
| `/aesth:status` | Show current design system state |
| `/aesth:validate <files>` | Validate code against design rules |
| `/aesth:capture` | Store patterns/decisions to Subcog |
| `/aesth:extract <files>` | Extract patterns from existing code |

## Architecture

### Memory-First Design

Aesth uses Subcog as the exclusive source of truth. No file-based fallback.

**Namespace**: `patterns`

**Domain Scoping**:
- `domain: project` — Project-specific tokens, patterns, and decisions
- `domain: user` — Universal craft principles shared across all projects

### Memory Types

| Type | Tags | Purpose |
|------|------|---------|
| **Design Direction** | `aesth`, `design-direction` | Personality, foundation, depth strategy |
| **Design Tokens** | `aesth`, `design-tokens` | Spacing, colors, typography, radius |
| **Component Patterns** | `aesth`, `component-pattern` | Button, Card, Input specifications |
| **Design Decisions** | `aesth`, `design-decision` | Choices with rationale and dates |
| **Craft Principles** | `aesth`, `craft-principles` | Universal rules (user domain) |

## Scope

**Use for**: Dashboards, admin panels, SaaS apps, tools, settings pages, data interfaces

**Not for**: Landing pages, marketing sites, campaigns (use `/frontend-design` instead)

## Quick Start

### New Project

```
/aesth:init

# Start building UI
"Build a settings page for user preferences"

# Aesth will:
# 1. Assess context
# 2. Suggest direction (e.g., "Precision & Density, borders-only")
# 3. Get your confirmation
# 4. Build with craft principles
# 5. Offer to save patterns to Subcog
```

### Existing Project

```
/aesth:init

# If design system exists in Subcog:
# → Loads and applies established patterns
# → Validates new code automatically

/aesth:status
# → Shows current Direction, Tokens, Patterns, Decisions
```

### Extract from Code

```
/aesth:extract src/components

# Scans existing code for:
# - Spacing values → infers scale
# - Colors → extracts palette
# - Component patterns → identifies buttons, cards, etc.
# - Depth strategy → counts borders vs shadows

# Then offers to save to Subcog
```

## Design Directions

Aesth supports 6 design personalities:

| Direction | Feel | Best For |
|-----------|------|----------|
| **Precision & Density** | Tight, technical, monochrome | Developer tools, admin dashboards |
| **Warmth & Approachability** | Generous spacing, soft shadows | Collaborative tools, consumer apps |
| **Sophistication & Trust** | Cool tones, layered depth | Finance, enterprise B2B |
| **Boldness & Clarity** | High contrast, dramatic space | Modern dashboards, data-heavy apps |
| **Utility & Function** | Muted, functional density | GitHub-style tools |
| **Data & Analysis** | Chart-optimized, numbers-first | Analytics, BI tools |

## Philosophy

**Decisions compound.**
A spacing value chosen once becomes a pattern. A depth strategy becomes an identity.

**Consistency beats perfection.**
A coherent system with "imperfect" values beats a scattered interface with "correct" ones.

**Memory enables iteration.**
When you can see what you decided and why, you can evolve intentionally instead of drifting accidentally.

**Subcog enables intelligence.**
Semantic search surfaces relevant patterns. Cross-project learning compounds knowledge.

## Differences from design-engineer

Aesth is inspired by [claude-design-engineer](https://github.com/Dammyjay93/claude-design-engineer) but reimagined for Subcog:

| Aspect | design-engineer | aesth |
|--------|-----------------|-------|
| Memory | `.design-engineer/system.md` file | Subcog MCP memories |
| Validation | Node.js script | Prompt-based hook with LLM reasoning |
| Cross-project | Copy files manually | User domain memories shared automatically |
| Search | Parse markdown file | Semantic search via Subcog |
| Capture | Manual file editing | `subcog_capture` with structured tags |

## Requirements

- Claude Code with plugin support
- Subcog MCP server configured and running

## Troubleshooting

**"No design system found" after init**
- This is normal for new projects. Start building UI and aesth will suggest a direction.
- For existing projects, run `/aesth:extract src/components` to pull patterns from code.

**Commands fail or timeout**
- Verify Subcog MCP server is running: `/subcog:status`
- Check your `.mcp.json` configuration includes the subcog server
- Aesth has no file-based fallback — Subcog is required

**Validation reports no rules**
- Run `/aesth:init` first to load design system
- If no system exists, establish one by building UI or running `/aesth:extract`

**Patterns not persisting between sessions**
- Confirm you saved patterns when prompted ("Want me to save these patterns to Subcog?")
- Check Subcog memory with `/subcog:status` to verify memories exist

## License

MIT

---

**Version**: 0.1.0
**Author**: Robert Allen
**Repository**: https://github.com/zircote/aesth
