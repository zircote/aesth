---
name: aesth:status
description: Show current design system state from Subcog including direction, tokens, patterns, and decisions.
allowed-tools: mcp__plugin_subcog_subcog__subcog_recall
---

# aesth status

Display current design system from Subcog memory.

## Implementation

Query Subcog for all project design memories:
```text
subcog_recall(filter: "ns:patterns tag:aesth domain:project", limit: 20)
```

## Output Format

**If design system exists:**

```text
Design System: [Project Name]

DIRECTION
  Personality: [Precision & Density / Warmth & Approachability / etc]
  Foundation: [Cool Slate / Warm Stone / etc]
  Depth: [Borders-only / Subtle shadows / Layered]
  Source: subcog://project/patterns/{urn}

TOKENS
  Spacing: [base]px base (scale: 4, 8, 12, 16, 24, 32, 48)
  Colors: [foundation] foundation, [accent] accent
  Typography: [font] system, [mono] code
  Radius: [scale]
  Source: subcog://project/patterns/{urn}

PATTERNS ([count])
  - Button Primary: [height]h, [padding]px, [radius] radius
  - Card Default: [depth], [padding] pad
  - [other patterns...]
  Source: subcog://project/patterns/{urn}

DECISIONS ([count])
  - [date]: [decision summary]
  - [date]: [decision summary]
  Source: subcog://project/patterns/{urn}
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
