# Aesth Architecture

Technical architecture and design decisions for the aesth plugin.

## Overview

Aesth is a Claude Code plugin that provides craft-focused design system management using Subcog MCP for persistent memory. It helps build interfaces with consistency by storing and recalling design decisions across sessions.

```
┌─────────────────────────────────────────────────────────────┐
│                      Claude Code                            │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Commands   │  │    Skills    │  │    Hooks     │      │
│  │  /aesth:*    │  │    aesth     │  │  PostToolUse │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                 │                 │               │
│         └─────────────────┼─────────────────┘               │
│                           │                                 │
│                    ┌──────▼───────┐                         │
│                    │  Subcog MCP  │                         │
│                    │   Server     │                         │
│                    └──────┬───────┘                         │
│                           │                                 │
│                    ┌──────▼───────┐                         │
│                    │   Memories   │                         │
│                    │  (patterns)  │                         │
│                    └──────────────┘                         │
└─────────────────────────────────────────────────────────────┘
```

## Core Design Decisions

### Memory-First Architecture

**Decision**: Use Subcog MCP as the exclusive source of truth. No file-based fallback.

**Rationale**:
- Semantic search enables intelligent pattern matching
- Cross-project learning via user-domain memories
- Consistent API for all memory operations
- Eliminates sync issues between files and memory

**Trade-offs**:
- Requires Subcog MCP to be running
- No offline fallback
- Initial setup complexity

### Consolidated Memory Granularity

**Decision**: Store 4-5 consolidated memories per project instead of many granular ones.

**Memory Types**:
1. **Design Direction** - Personality, foundation, depth strategy
2. **Design Tokens** - Spacing, colors, typography, radius
3. **Component Patterns** - Button, Card, Input specifications
4. **Design Decisions** - Choices with rationale and dates
5. **Craft Principles** - Universal rules (user domain, shared)

**Rationale**:
- Easier to query and reason about
- Reduces memory fragmentation
- Clear separation of concerns
- Efficient recall operations

### Domain Scoping

**Decision**: Use `domain: project` for project-specific patterns, `domain: user` for universal principles.

| Content | Domain | Scope |
|---------|--------|-------|
| Design direction | project | Single project |
| Design tokens | project | Single project |
| Component patterns | project | Single project |
| Design decisions | project | Single project |
| Craft principles | user | All projects |

**Benefits**:
- Project isolation prevents cross-contamination
- Universal principles compound across all work
- Clear ownership and lifecycle

## Component Architecture

### Plugin Manifest

`plugin.json` defines the plugin entry points:

```json
{
  "name": "aesth",
  "commands": "./.claude/commands",
  "skills": "./.claude/skills"
}
```

### Commands

Located in `.claude/commands/`, each command is a markdown file with:
- YAML frontmatter (name, description)
- Instructions for Claude to execute

| Command | Purpose |
|---------|---------|
| `/aesth:init` | Load design system from Subcog |
| `/aesth:status` | Display current design system |
| `/aesth:validate` | Check code against design rules |
| `/aesth:capture` | Store patterns to Subcog |
| `/aesth:extract` | Extract patterns from existing code |

### Skills

The main skill (`SKILL.md`) provides:
- Core design principles
- Subcog integration patterns
- Workflow guidelines
- Communication style rules

Reference materials in `references/` provide detailed specifications.

## Data Flow

### Initialization Flow

```
/aesth:init
    │
    ▼
subcog_recall(filter: "ns:patterns tag:aesth tag:design-direction domain:project")
    │
    ├─── Memories exist ───► Load and display direction
    │
    └─── No memories ──────► Ready to establish
```

### Build Flow (New Project)

```
User requests UI build
    │
    ▼
Assess context (product type, users, purpose)
    │
    ▼
Suggest direction + ask for confirmation
    │
    ▼
Build with craft principles
    │
    ▼
Offer to save patterns to Subcog
    │
    ▼
subcog_capture(namespace: "patterns", tags: ["aesth", ...], domain: "project")
```

### Validation Flow

```
Write/Edit UI file
    │
    ▼
PostToolUse hook triggers
    │
    ▼
Query Subcog for:
  - Design tokens
  - Craft principles
  - Design direction
    │
    ▼
Validate code against rules:
  - Spacing grid compliance
  - Depth strategy consistency
  - Color palette adherence
  - Pattern matching
    │
    ▼
Report violations with specific fixes
```

## Subcog Integration

### Query Patterns

```text
# Project design direction
subcog_recall(filter: "ns:patterns tag:aesth tag:design-direction domain:project")

# Project tokens
subcog_recall(filter: "ns:patterns tag:aesth tag:design-tokens domain:project")

# Universal craft principles
subcog_recall(filter: "ns:patterns tag:aesth tag:craft-principles domain:user")
```

### Capture Patterns

```text
subcog_capture(
  content: "...",
  namespace: "patterns",
  tags: ["aesth", "design-direction", "precision-density"],
  domain: "project"
)
```

### Tag Conventions

| Tag | Purpose |
|-----|---------|
| `aesth` | All aesth memories |
| `design-direction` | Direction memories |
| `design-tokens` | Token memories |
| `component-pattern` | Component specifications |
| `design-decision` | Recorded decisions |
| `craft-principles` | Universal principles |
| `{personality}` | Direction personality (e.g., `precision-density`) |
| `{component-name}` | Specific component (e.g., `button`, `card`) |

## Design Directions

Six supported design personalities:

| Direction | Characteristics | Best For |
|-----------|-----------------|----------|
| Precision & Density | Tight spacing, monochrome, borders | Developer tools, admin |
| Warmth & Approachability | Generous spacing, soft shadows | Collaborative tools |
| Sophistication & Trust | Cool tones, layered depth | Finance, enterprise |
| Boldness & Clarity | High contrast, dramatic space | Modern dashboards |
| Utility & Function | Muted, functional density | GitHub-style tools |
| Data & Analysis | Chart-optimized, numbers-first | Analytics, BI |

## Validation Rules

### Spacing Grid

- All values must be multiples of base unit (typically 4px)
- Standard scale: 4, 8, 12, 16, 24, 32, 48px
- Border widths (1-2px) exempt from grid

### Depth Strategy

Three mutually exclusive approaches:
1. **Borders-only**: No shadows, use borders for definition
2. **Subtle shadows**: Single layer, soft lift
3. **Layered shadows**: Multiple layers for premium feel

### Color Palette

- Gray for structure
- Color only for meaning (status, action, emphasis)
- Must match defined palette

### Typography

- Headlines: 600 weight, tight tracking
- Body: 400-500 weight, standard tracking
- Data: Monospace, tabular-nums

## Extension Points

### Adding Commands

1. Create `.claude/commands/newcommand.md`
2. Add YAML frontmatter with name and description
3. Write clear instructions
4. Test with various scenarios

### Adding Validation Rules

1. Update validation hook prompt
2. Add Subcog query for new rule type
3. Define violation detection logic
4. Provide fix suggestions

### Adding Design Directions

1. Add direction to `references/principles.md`
2. Update SKILL.md direction list
3. Define characteristic patterns
4. Add tag convention

## Limitations

- Requires Subcog MCP server
- No offline mode
- Validation is LLM-based (not static analysis)

## Future Considerations

- Static analysis validation option
- Design token export (CSS variables, Tailwind config)
- Component library generation
- Visual diff reporting
