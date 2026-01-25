---
name: aesth
description: Interface design with craft and consistency. For dashboards, apps, tools — not marketing sites. Uses Subcog for persistent design memory.
---

# Aesth: Craft-Focused Design

Build interface design with elegance, consistency, and memory.

## Mnemonic Integration

Before starting any design work, recall relevant memories:

```bash
# Search for design-related memories
rg -i "design|aesth|ui|pattern|spacing|color" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -10

# Check decisions namespace
rg -l "." ~/.claude/mnemonic/*/decisions/ --glob "*.memory.md" 2>/dev/null | xargs -I {} basename {} 2>/dev/null | head -5
```

Apply recalled context:
- User's preferred design personality across projects
- Patterns that have been successful
- Decisions about depth, spacing, and color strategies

## Scope

**Use for:** Dashboards, admin panels, SaaS apps, tools, settings pages, data interfaces.

**Not for:** Landing pages, marketing sites, campaigns. Redirect those to `/frontend-design`.

---

# Subcog Integration

Aesth uses Subcog MCP for all design system memory. No file-based system.md.

## Session Start

Query Subcog for existing project design system:

```text
subcog_recall(filter: "ns:patterns tag:aesth tag:design-direction domain:project")
```

If memories exist, apply established patterns. If not, ready to establish.

## Memory Structure

Design system stored as 4-5 consolidated memories in `patterns` namespace:

1. **Design Direction** - Personality, Foundation, Depth strategy
   - Tags: `["aesth", "design-direction", "{personality}"]`
   - Domain: `project`

2. **Design Tokens** - Spacing, Colors, Typography, Radius
   - Tags: `["aesth", "design-tokens"]`
   - Domain: `project`

3. **Component Patterns** - Button, Card, Input specifications
   - Tags: `["aesth", "component-pattern", "{component-name}"]`
   - Domain: `project`

4. **Design Decisions** - Choices with rationale and dates
   - Tags: `["aesth", "design-decision"]`
   - Domain: `project`

5. **Craft Principles** - Universal rules (shared across projects)
   - Tags: `["aesth", "craft-principles", "universal"]`
   - Domain: `user`

---

# Before Writing Code

State your design choices first. This keeps thinking deliberate.

```text
Direction: [what this should feel like]
Depth: [borders / subtle shadows / layered]
Spacing: [base unit]
```

Then write the code.

---

# Design Principles

## Spacing
Pick a base unit (4px) and stick to multiples. Random values signal no system.

## Padding
Keep it symmetrical. If one side is 16px, others should match unless there's a clear reason.

## Depth
Choose ONE approach and commit:
- **Borders-only** — Clean, technical. For dense tools.
- **Subtle shadows** — Soft lift. For approachable products.
- **Layered shadows** — Premium, dimensional. For cards that need presence.

Don't mix approaches.

## Border Radius
Sharper feels technical. Rounder feels friendly. Pick a scale and apply consistently.

## Typography
Headlines need weight and tight tracking. Body needs readability. Data needs monospace. Build a hierarchy.

## Color
Gray builds structure. Color communicates meaning — status, action, emphasis. Decorative color is noise.

## Animation
Fast micro-interactions (~150ms), smooth easing. No bouncy/spring effects.

## Controls
Native `<select>` and `<input type="date">` can't be styled. Build custom components.

---

# Avoid

- Dramatic drop shadows
- Large radius on small elements
- Pure white cards on colored backgrounds
- Thick decorative borders
- Excessive spacing (>48px margins)
- Gradients for decoration
- Multiple accent colors

---

# Self-Check

Before finishing:
- Did I think about what this product needs, or default?
- Is my depth strategy consistent throughout?
- Does every element feel intentional?

The standard: looks designed by someone who obsesses over details.

---

# After Completing a Task

When you finish building something, **always offer to save**:

```text
"Want me to save these patterns to Subcog for future sessions?"
```

If yes, use `subcog_capture` to store:
- Direction and foundation (`namespace: patterns`, `domain: project`)
- Depth strategy
- Spacing base unit and scale
- Key component patterns with specific values

Tags should include: `["aesth", "{memory-type}"]`

This compounds — each save makes future work faster and more consistent.

### Also Capture to Mnemonic

For design decisions that represent cross-project learnings:

```bash
# Check if insight already exists
rg -i "{decision-topic}" ~/.claude/mnemonic/ --glob "*.memory.md"

# If novel, capture to mnemonic
UUID=$(uuidgen | tr '[:upper:]' '[:lower:]')
DATE=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
TITLE="Design Decision: {topic}"
SLUG=$(echo "$TITLE" | tr '[:upper:]' '[:lower:]' | sed 's/[^a-z0-9]/-/g' | sed 's/--*/-/g' | head -c 50)

cat > ~/.claude/mnemonic/default/decisions/user/${UUID}-${SLUG}.memory.md << 'MEMORY'
---
id: ${UUID}
type: semantic
namespace: decisions/user
created: ${DATE}
modified: ${DATE}
title: "${TITLE}"
tags:
  - aesth
  - design-decision
  - {specific-tags}
temporal:
  valid_from: ${DATE}
  recorded_at: ${DATE}
provenance:
  source_type: design-session
  agent: claude-opus-4
  confidence: 0.85
---

# {Decision Title}

## Decision
{What was decided}

## Rationale
{Why this decision was made}

## Impact
{How this affects future design work}
MEMORY
```

---

# Workflow

## Communication
Be invisible. Don't announce modes or narrate process.

**Never say:** "I'm in ESTABLISH MODE", "Let me check Subcog..."

**Instead:** Jump into work. State suggestions with reasoning.

## Suggest + Ask
Lead with your recommendation, then confirm:
```text
"This feels like a data-heavy admin tool — I'd go minimal.
Tight spacing, monochrome, borders for depth."

[AskUserQuestion: "Does that direction feel right?"]
```

## If Project Has Subcog Memories
Query Subcog patterns namespace with project tags. Apply established patterns.

## If No Subcog Memories
1. Assess context — What's the product? Who uses it?
2. Suggest + ask — State recommendation, get confirmation
3. Build — Apply principles
4. Offer to save to Subcog

---

# Deep Dives

For more detail on specific topics:
- `references/principles.md` — Code examples, specific values, dark mode
- Query Subcog: `subcog_recall(filter: "ns:patterns tag:craft-principles domain:user")`

# Commands

- `/aesth:init` — Initialize skill and load design system
- `/aesth:status` — Show current design system from Subcog
- `/aesth:validate` — Check code against Subcog design rules
- `/aesth:capture` — Store patterns/decisions to Subcog
- `/aesth:extract` — Extract patterns from code, save to Subcog
