---
name: extract
description: Extract design patterns from existing codebase and capture to Subcog memory.
allowed-tools: mcp__plugin_subcog_subcog__subcog_capture, mcp__plugin_subcog_subcog__subcog_recall, Read, Glob, Grep, Bash, Write
---

## Memory

Search first: `rg -i "extract design" ~/.claude/mnemonic/ ./.claude/mnemonic/ --glob "*.memory.md"`
Capture after: `/mnemonic:capture learnings "{title}"`

# aesth extract

Scan existing code for design patterns and capture to Subcog.

## Usage

```
/aesth:extract src/components
/aesth:extract src/**/*.tsx
/aesth:extract                  # Extracts from common UI paths
```

## Process

1. **Scan Files**: Read CSS/JSX/styled-components for design values

2. **Extract Patterns**:
   - **Spacing**: Cluster px values into scale
   - **Colors**: Extract color palette (CSS vars, hex, rgb)
   - **Typography**: Identify font-sizes, line-heights, weights
   - **Radius**: Detect border-radius values
   - **Components**: Identify patterns (button, card, input measurements)
   - **Depth**: Count shadows vs borders to infer strategy

3. **Infer Direction**:
   - Tight spacing → Precision & Density
   - Generous spacing → Warmth & Approachability
   - Borders-only → Technical aesthetic
   - Cool grays → Professional tone
   - Warm grays → Approachable tone

4. **Capture to Subcog**:
   - Design Tokens (consolidated)
   - Component Patterns (each unique pattern)
   - Inferred Direction (with confidence note)

## Extraction Details

### Spacing Analysis
```
Found: 4px (12x), 8px (23x), 12px (18x), 16px (31x), 24px (8x)
→ Suggests: Base 4px, Scale: 4, 8, 12, 16, 24
Outliers: 14px (2x), 20px (1x) - may need alignment
```

### Color Analysis
```
Foundation colors:
- #f8fafc (12x) → slate-50
- #f1f5f9 (8x) → slate-100
- #475569 (15x) → slate-600
- #0f172a (6x) → slate-900

Accent colors:
- #3b82f6 (18x) → blue-600 (likely primary)

Status colors:
- #10b981 (4x) → green-500 (success)
- #ef4444 (3x) → red-500 (error)
```

### Radius Analysis
```
Found: 6px (28x), 8px (5x), 4px (12x)
→ Suggests: Scale: 4px, 6px, 8px (sharp/technical)
```

### Depth Analysis
```
box-shadow found: 2 instances
border found: 34 instances
→ Suggests: Borders-only depth strategy (94% border usage)
```

### Component Analysis
```
Button patterns found (8 instances):
- Height: 36px (7/8), 40px (1/8)
- Padding: 12px 16px (6/8), 16px (2/8)
→ Suggests: Button Primary: 36px h, 12px 16px pad

Card patterns found (12 instances):
- Border: 1px solid (10/12), none (2/12)
- Padding: 16px (9/12), 20px (3/12)
→ Suggests: Card Default: 1px border, 16px pad
```

## Output Format

```
Extracted from: src/components (12 files)

SPACING SCALE (detected)
  Common values: 4, 8, 12, 16, 24, 32, 48
  Base: 4px (97% adherence)
  Outliers: 14px (2 instances), 20px (1 instance)

COLORS (detected)
  Foundation: Slate (#f1f5f9 to #0f172a)
  Accent: Blue (#3b82f6, #2563eb)
  Status: Green (#10b981), Red (#ef4444)

TYPOGRAPHY (detected)
  System: Inter (inferred from font-family)
  Sizes: 12, 14, 16, 18, 20px
  Weights: 400 (regular), 500 (medium), 600 (semibold)

RADIUS (detected)
  Common: 6px, 8px
  Scale: 4/6/8 (technical)

COMPONENTS (detected)
  - Button Primary: 36px h, 12px/16px px, 6px radius
  - Card: 16px pad, 8px radius, border
  - Input: 36px h, 12px px, border focus

INFERRED DIRECTION
  Personality: Precision & Density (tight spacing, technical)
  Foundation: Cool Slate (gray palette)
  Depth: Borders-only (no shadows detected)
  Confidence: High (patterns consistent)

Capture to Subcog? (y/n/customize)
```

## After Confirmation

If user confirms, capture:
1. **Design Direction** - Inferred personality, foundation, depth
2. **Design Tokens** - Extracted spacing, colors, typography, radius
3. **Component Patterns** - One memory per detected component
4. **Design Decisions** - Extraction summary as initial decision

All tagged with: `["aesth", "extracted", "{memory-type}"]`

## Customization

If user selects "customize":
- Present each extracted value for confirmation
- Allow adjustments before capture
- Mark as "extracted-customized" in tags

## Common UI Paths

When invoked without path argument, scan:
- `src/components/**/*.{tsx,jsx,css,scss}`
- `app/components/**/*.{tsx,jsx,css,scss}`
- `components/**/*.{tsx,jsx,css,scss}`
- `styles/**/*.{css,scss}`
