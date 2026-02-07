---
name: aesth:capture
description: Capture design patterns, tokens, or decisions to Subcog memory.
allowed-tools: mcp__plugin_subcog_subcog__subcog_capture, mcp__plugin_subcog_subcog__subcog_recall, Bash, Glob, Grep, Read, Write
---

# aesth capture

Manually store design patterns, decisions, or tokens to Subcog.

## Usage

Interactive mode - prompts for what to capture:
```
/aesth:capture
```

Direct capture with type:
```
/aesth:capture direction
/aesth:capture tokens
/aesth:capture pattern button
/aesth:capture decision
```

## Capture Types

### 1. Design Direction

Capture the overall design personality and strategy.

**Content Structure:**
```markdown
## Design Direction

Personality: [Precision & Density | Warmth & Approachability | Sophistication & Trust | Boldness & Clarity | Utility & Function | Data & Analysis]
Foundation: [Cool Slate | Warm Stone | Pure Neutral | Tinted]
Depth: [Borders-only | Subtle shadows | Layered shadows]

Rationale:
- [Why this personality fits the product]
- [Context about users and their needs]
- [Key considerations that drove the decision]

Established: [ISO 8601 date]
```

**Subcog Parameters:**
- Namespace: `patterns`
- Domain: `project`
- Tags: `["aesth", "design-direction", "{personality-slug}", "{depth-slug}"]`

---

### 2. Design Tokens

Capture spacing, colors, typography, and radius scales.

**Content Structure:**
```markdown
## Design Tokens

### Spacing Scale
Base: 4px
Scale: 4, 8, 12, 16, 24, 32, 48, 64

### Colors
Foundation: Slate
- slate-50: #f8fafc
- slate-100: #f1f5f9
- slate-600: #475569
- slate-900: #0f172a

Accent: Blue
- blue-500: #3b82f6
- blue-600: #2563eb

Status:
- success: #10b981
- warning: #f59e0b
- error: #ef4444

### Typography
System: Inter (sans-serif)
Code: SF Mono (monospace)

Scale:
- xs: 12px / 1.4
- sm: 14px / 1.5
- base: 16px / 1.5
- lg: 18px / 1.6

### Border Radius
Scale: 4px, 6px, 8px

Updated: [ISO 8601 date]
```

**Subcog Parameters:**
- Namespace: `patterns`
- Domain: `project`
- Tags: `["aesth", "design-tokens", "spacing", "colors", "typography", "radius"]`

---

### 3. Component Pattern

Capture specific component measurements and styles.

**Content Structure:**
```markdown
## Component: [Name]

### Variant: Primary
- Height: 36px
- Padding: 12px 16px
- Border Radius: 6px
- Background: blue-600
- Color: white
- Font: 14px medium
- Hover: blue-700

### Variant: Secondary
- Height: 36px
- Padding: 12px 16px
- Border: 1px solid slate-300
- Background: white
- Color: slate-700
- Hover: slate-50 bg

Rationale:
- [Why these measurements]
- [Usage guidelines]

Created: [ISO 8601 date]
```

**Subcog Parameters:**
- Namespace: `patterns`
- Domain: `project`
- Tags: `["aesth", "component-pattern", "{component-name}"]`

---

### 4. Design Decision

Capture a specific design choice with rationale.

**Content Structure:**
```markdown
## Decision: [Topic]

### Choice
[What was decided]

### Context
[Why this decision was needed]

### Alternatives Considered
- [Option 1]: Rejected because...
- [Option 2]: Rejected because...

### Impact
[What this affects going forward]

Date: [ISO 8601 date]
```

**Subcog Parameters:**
- Namespace: `patterns`
- Domain: `project`
- Tags: `["aesth", "design-decision", "{topic-slug}"]`

---

## Output

After successful capture:
```text
Captured to Subcog: Design Direction
URN: subcog://project/patterns/aesth-direction-a1b2c3d4
Tags: aesth, design-direction, precision-density, borders-only

Content preview:
  ## Design Direction
  Personality: Precision & Density
  Foundation: Cool Slate
  ...
```

## Interactive Mode

When invoked without arguments, prompt:

```text
What would you like to capture?

1. Direction - Overall design personality and depth strategy
2. Tokens - Spacing, colors, typography scales
3. Pattern - Component-specific measurements
4. Decision - Design choice with rationale

Select (1-4):
```

Then gather relevant information through conversation.

---

## Mnemonic Integration: Cross-Session Learning

After capturing design patterns to Subcog, also capture significant decisions to mnemonic for cross-project learning.

### When to Capture to Mnemonic

Capture to mnemonic when the design decision represents:
- A reusable design pattern applicable across projects
- A lesson learned from design iteration
- A craft principle discovered through practice

### Capture Process

After successful Subcog capture, if the content qualifies:

```bash
# Search for related existing memories first
rg -i "design|pattern|aesth" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5

# If this is a novel insight, capture it
UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
DATE=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
NAMESPACE="patterns"
TITLE="Design Pattern: {pattern-name}"
SLUG=$(echo "$TITLE" | tr '[:upper:]' '[:lower:]' | sed 's/[^a-z0-9]/-/g' | sed 's/--*/-/g' | head -c 50)

cat > ~/.claude/mnemonic/default/${NAMESPACE}/user/${UUID}-${SLUG}.memory.md << 'MEMORY'
---
id: ${UUID}
type: procedural
namespace: patterns/user
created: ${DATE}
modified: ${DATE}
title: "${TITLE}"
tags:
  - aesth
  - design-pattern
  - {specific-tags}
temporal:
  valid_from: ${DATE}
  recorded_at: ${DATE}
provenance:
  source_type: design-capture
  agent: claude-opus-4
  confidence: 0.85
---

# {Pattern Title}

## Level 1: Quick Answer
{One-line summary of the pattern}

## Level 2: Context
{When and why to use this pattern}

## Level 3: Full Detail
{Complete pattern specification with rationale}
MEMORY
```

### Do Not Capture to Mnemonic

- Project-specific tokens (colors, spacing) - these stay in Subcog only
- Routine component patterns without novel insight
- Decisions that only apply to current project context
