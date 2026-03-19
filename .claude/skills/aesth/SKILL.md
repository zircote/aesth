---
name: aesth
description: Interface design with craft and consistency. For dashboards, apps, tools — not marketing sites. Uses Subcog for persistent design memory.
---

## Memory

Search first: `rg -i "design aesth" ~/.claude/mnemonic/ ./.claude/mnemonic/ --glob "*.memory.md"`
Capture after: `/mnemonic:capture patterns "{title}"`

# Aesth: Craft-Focused Design

Build interface design with elegance, consistency, and memory.

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
**Every spacing, padding, margin, gap, width, and height value must be a multiple of 4px.** No exceptions. No 2px, no 6px, no 10px, no 14px. The minimum spacing value is 4px. If something feels like it needs 2px, use 4px. If it feels like it needs 6px, use 4px or 8px. Random or off-grid values signal no system.

**Why the 4px grid matters**: A strict grid creates visual rhythm — elements align predictably, white space feels intentional rather than accidental. It ensures consistent spacing across every screen without per-case judgment calls, makes scaling between densities trivial (just multiply), and reduces visual noise from micro-misalignments that erode perceived quality. Off-grid values break this rhythm and make the interface feel uncoordinated.

## Padding
Keep it symmetrical. If one side is 16px, others should match unless there's a clear reason.

## Depth
Choose ONE approach and commit. **Match to product type:**

- **Borders-only** — Clean, technical. **Use for:** financial tools, data-heavy dashboards, admin panels, analytics, enterprise software. When accuracy and trust matter more than personality.
- **Subtle shadows** — Soft lift. **Use for:** SaaS products, collaboration tools, approachable consumer apps. When the product should feel friendly but professional.
- **Layered shadows** — Premium, dimensional. **Use for:** marketing-adjacent product UIs, portfolio/showcase tools, creative apps. **NOT for** financial, data-heavy, or enterprise tools — layered shadows undermine the precision those products need.

Don't mix approaches. When in doubt for data/financial/enterprise products, default to borders-only.

## Border Radius
Sharper feels technical. Rounder feels friendly. The valid range is **4px to 16px** — any border-radius above 16px is an anti-pattern regardless of element size. Pick a scale within this range and apply consistently.

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
- Border-radius above 16px (24px+ is never appropriate in craft design — use the 4-16px range)
- Pure white cards on colored backgrounds
- Thick decorative borders
- Excessive spacing (>48px margins)
- Gradients for decoration
- Multiple accent colors

---

# Reviewing & Validating Code

When reviewing or validating code against design rules, **always explain WHY a rule matters**, not just that it was violated. Don't just say "this is off-grid, fix it." Explain the design reasoning so the developer understands the principle and can apply it independently.

For example, when flagging a 4px grid violation, explain that grid alignment creates visual rhythm, ensures consistent spacing across screens, makes density scaling straightforward, and reduces visual noise from micro-misalignments. When flagging depth strategy mixing, explain why consistency in elevation creates a coherent spatial model. Every correction should teach, not just enforce.

---

# Self-Check

Before finishing:
- Did I think about what this product needs, or default?
- Is my depth strategy consistent throughout and appropriate for the product type? (financial/data = borders-only or subtle, NOT layered)
- Does every element feel intentional?
- Are ALL spacing/padding/margin/gap/size values multiples of 4px? Scan for any 2px, 6px, 10px, 14px violations — these MUST be corrected.
- When flagging violations, did I explain WHY the rule exists, not just state it?

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
