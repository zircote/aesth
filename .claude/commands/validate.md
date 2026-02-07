---
name: validate
description: Validate code files against design tokens, craft principles, and design direction stored in Subcog.
allowed-tools: mcp__plugin_subcog_subcog__subcog_recall, Read, Glob, Bash, Grep, Write
---

# aesth validate

Check code for design consistency violations.

## Pre-Validation: Recall Validation Learnings from Mnemonic

Before validating, check mnemonic for prior validation insights:

```bash
# Search for validation-related memories
rg -i "validation|violation|design-check" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5
```

Apply recalled insights:
- Common violation patterns to watch for
- False positives to avoid flagging
- Context-specific exceptions learned from prior validations

## Usage

```
/aesth:validate src/components/Button.tsx
/aesth:validate src/components/**/*.tsx
/aesth:validate                           # Validates common UI paths
```

## Process

1. **Query Subcog for Rules**:
   ```
   # Project tokens
   subcog_recall(filter: "ns:patterns tag:aesth tag:design-tokens domain:project")

   # Craft principles (universal)
   subcog_recall(filter: "ns:patterns tag:aesth tag:craft-principles domain:user")

   # Design direction
   subcog_recall(filter: "ns:patterns tag:aesth tag:design-direction domain:project")
   ```

2. **Read Target Files**: Load file content

3. **Validate Against Rules**: Check for:
   - Spacing values not in scale
   - Colors not in palette
   - Border radius off scale
   - Typography outside hierarchy
   - Depth strategy conflicts
   - Padding asymmetry
   - Craft principle violations

4. **Output Results**: Violations with fixes

## Validation Checks

### Spacing Grid
- Extract all `Npx` values from code
- Check if values are multiples of spacing base
- Flag off-grid values (allow 1-2px for borders)

### Depth Consistency
- If system is `borders-only` → flag any `box-shadow`
- If system is `subtle-shadows` → flag layered shadows
- Allow ring shadows (`0 0 0 1px`) in borders-only systems

### Color Palette
- Extract hex colors from code
- Check against defined palette
- Flag unknown colors

### Pattern Matching
- Find buttons not matching Button pattern
- Find cards not matching Card pattern
- Report measurement differences

## Output Format

```text
Validation: src/components/Button.tsx

VIOLATIONS (3)

  Line 24: padding: 14px 20px
    Issue: Off-scale spacing values
    Scale: 4, 8, 12, 16, 24, 32, 48
    Fix: padding: 12px 16px

  Line 31: box-shadow: 0 4px 12px rgba(0,0,0,0.15)
    Issue: Shadow conflicts with borders-only depth strategy
    Strategy: Borders-only (no shadows)
    Fix: Remove shadow, use border: 1px solid var(--slate-300)

  Line 18: border-radius: 5px
    Issue: Off-scale radius
    Scale: 4px, 6px, 8px
    Fix: border-radius: 6px

PASSED (7)
  - Spacing: 16px padding ✓
  - Color: var(--blue-600) ✓
  - Typography: 14px font-size ✓
  - Border radius: 6px (line 32) ✓
  - Height: 36px ✓
  - Padding symmetry ✓
  - Semantic color usage ✓
```

## If No Design System

```text
No design system in Subcog to validate against.

Create a system first:
1. Build UI → establish system automatically
2. Run /aesth:extract → create system from existing code
```

---

## Post-Validation: Capture Insights to Mnemonic

If validation reveals novel insights or recurring patterns, capture to mnemonic:

```bash
# Check if similar insight exists
rg -i "{violation-type}" ~/.claude/mnemonic/ --glob "*.memory.md"

# Capture novel validation learnings
UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
DATE=$(date -u +"%Y-%m-%dT%H:%M:%SZ")

cat > ~/.claude/mnemonic/default/learnings/user/${UUID}-validation-insight.memory.md << 'MEMORY'
---
id: ${UUID}
type: episodic
namespace: learnings/user
created: ${DATE}
modified: ${DATE}
title: "Validation Learning: {Issue Type}"
tags:
  - aesth
  - validation
  - design-consistency
temporal:
  valid_from: ${DATE}
  recorded_at: ${DATE}
provenance:
  source_type: validation-session
  agent: claude-opus-4
  confidence: 0.8
---

# Validation Learning

## Pattern Identified
{Description of the common violation or insight}

## Root Cause
{Why this pattern occurs}

## Prevention
{How to avoid this in future work}
MEMORY
```
