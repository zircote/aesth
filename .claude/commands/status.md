---
name: aesth:status
description: Show current design system state from Subcog including direction, tokens, patterns, and decisions.
allowed-tools: mcp__plugin_subcog_subcog__subcog_recall, Bash, Glob, Grep, Read
---

# aesth status

Display current design system from Subcog memory.

## Pre-Status: Check Mnemonic for Related Memories

Before displaying status, optionally check mnemonic for related design memories:

```bash
# Quick check for design-related memories
rg -c "aesth|design" ~/.claude/mnemonic/ --glob "*.memory.md" 2>/dev/null | head -3
```

This provides context about:
- Design decisions made in other projects that may inform current work
- Patterns that have been successful across projects
- User's evolving design preferences

## Implementation

Query Subcog for all project design memories:
```text
subcog_recall(filter: "ns:patterns tag:aesth domain:project", limit: 20)
```

## Output Format

**If design system exists:**

```text
Design System: my-dashboard-app

DIRECTION
  Personality: Precision & Density
  Foundation: Cool Slate
  Depth: Borders-only
  Source: subcog://project/patterns/aesth-direction-a1b2c3d4

TOKENS
  Spacing: 4px base (scale: 4, 8, 12, 16, 24, 32, 48)
  Colors: Slate foundation, Blue accent
  Typography: Inter system, SF Mono code
  Radius: 4px, 6px, 8px
  Source: subcog://project/patterns/aesth-tokens-e5f6g7h8

PATTERNS (3)
  - Button Primary: 36px h, 12px 16px pad, 6px radius
  - Card Default: border, 16px pad
  - Input: 36px h, 12px pad, border focus
  Source: subcog://project/patterns/aesth-patterns-i9j0k1l2

DECISIONS (2)
  - 2026-01-15: Chose borders-only for technical aesthetic
  - 2026-01-16: Added 48px to spacing scale for section gaps
  Source: subcog://project/patterns/aesth-decisions-m3n4o5p6
```

**If no design system:**

```text
No design system found in Subcog.

Next steps:
1. Start building UI → system will be established automatically
2. Run /aesth:extract → pull patterns from existing code
```

## Details to Display

Parse each memory type and format:

### Direction Memory
- Personality (one of 6 design directions)
- Foundation (color temperature)
- Depth strategy (borders/shadows/layered)

### Tokens Memory
- Spacing base and scale
- Color palette summary
- Typography settings
- Border radius scale

### Patterns Memory
- Component name and key measurements
- List format with primary values

### Decisions Memory
- Date and summary of each decision
- Count of total decisions
