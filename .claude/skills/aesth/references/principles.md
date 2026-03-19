---
name: principles
description: Quick reference for craft design principles
---

# Design Principles Reference

Quick reference for craft principles.

**Note**: The authoritative source lives in Subcog at `domain: user, tag: craft-principles`. This file is for quick lookup only.

---

## The 4px Grid — STRICT, NO EXCEPTIONS

All spacing, padding, margin, gap, width, and height values MUST be multiples of 4px. **The minimum value is 4px.** There are no exceptions, not even for small elements like badges, dots, or icons.

**Why this matters**: The 4px grid creates visual rhythm — elements snap to a predictable cadence that the eye recognizes as intentional. It ensures consistent spacing across every screen without per-case judgment calls, makes scaling between density modes trivial (just multiply the base), and eliminates the visual noise caused by micro-misalignments (e.g., 13px next to 15px). Off-grid values break this rhythm and make the interface feel uncoordinated, even if the viewer can't articulate why. When reviewing code, always explain this reasoning — don't just cite the rule.

**Allowed values**: 4px, 8px, 12px, 16px, 20px, 24px, 28px, 32px, 36px, 40px, 44px, 48px...

**Forbidden values**: 2px, 6px, 10px, 14px, 18px, 22px, 25px, 30px — anything not divisible by 4.

Scale reference:
- `4px` - micro spacing (icon gaps, minimum possible spacing)
- `8px` - tight spacing (within components, small badge padding)
- `12px` - standard spacing (between related elements)
- `16px` - comfortable spacing (section padding)
- `24px` - generous spacing (between sections)
- `32px` - major separation

**If something feels like it needs 2px, use 4px. If it feels like it needs 6px, use 4px or 8px. Never compromise the grid.**

```css
/* Good */
padding: 12px 16px;
margin-bottom: 24px;
gap: 8px;
/* Small badge — still on grid */
padding: 4px 8px;
/* Status dot — still on grid */
width: 8px; height: 8px;

/* Bad — ALL of these violate the grid */
padding: 2px 8px;   /* 2px is below minimum */
gap: 6px;            /* 6 is not a multiple of 4 */
width: 6px;          /* 6 is not a multiple of 4 */
padding: 14px 20px;  /* 14 is not a multiple of 4 */
margin-bottom: 25px; /* 25 is not a multiple of 4 */
```

---

## Symmetrical Padding

TLBR must match. If top padding is 16px, left/bottom/right must also be 16px. Exception: when horizontal needs more room.

```css
/* Good */
padding: 16px;
padding: 12px 16px; /* Vertical/horizontal variation */

/* Bad */
padding: 24px 16px 12px 16px; /* Asymmetric without reason */
```

---

## Border Radius Consistency

The valid border-radius range is **4px to 16px**. Any radius above 16px (e.g., 24px, 32px) is always an anti-pattern in craft design, regardless of element size. Sharper corners feel technical, rounder corners feel friendly. Pick a system within this range and commit:

- **Sharp (technical)**: 4px, 6px, 8px
- **Soft (friendly)**: 8px, 12px, 16px
- **Minimal**: 2px, 4px, 6px

Don't mix systems. Consistency creates coherence. **Always flag border-radius values above 16px during code review.**

---

## Depth & Elevation Strategy

Match your depth approach to your product type. Choose ONE and commit:

### Product Type → Depth Strategy Guide

| Product Type | Recommended Depth | Why |
|---|---|---|
| Financial tools, fintech, banking | Borders-only | Precision and trust; shadows feel frivolous |
| Data dashboards, analytics, BI | Borders-only | Dense information needs clean separation |
| Admin panels, enterprise tools | Borders-only or Subtle | Utility-first; minimal decoration |
| SaaS apps, collaboration tools | Subtle shadows | Approachable but professional |
| Consumer-facing apps | Subtle or Layered | Warmth and personality appropriate |
| Creative tools, portfolio apps | Layered shadows | Premium feel suits the context |

**Borders-only (flat)**
Clean, technical, dense. For financial tools, data-heavy dashboards, analytics, and enterprise software.
```css
--border: rgba(0, 0, 0, 0.08);
--border-subtle: rgba(0, 0, 0, 0.05);
border: 0.5px solid var(--border);
```

**Subtle single shadows**
Soft lift without complexity. For SaaS products and approachable tools.
```css
--shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
```

**Layered shadows**
Rich, premium, dimensional. For creative and consumer apps where personality matters. **Do NOT use for financial, data-heavy, or enterprise tools** — layered shadows undermine the precision and trust those products require.
```css
--shadow-layered:
  0 0 0 0.5px rgba(0, 0, 0, 0.05),
  0 1px 2px rgba(0, 0, 0, 0.04),
  0 2px 4px rgba(0, 0, 0, 0.03),
  0 4px 8px rgba(0, 0, 0, 0.02);
```

**Never mix approaches.** If you chose borders-only, no shadows anywhere.

---

## Typography Hierarchy

Build a clear hierarchy:

- **Headlines**: 600 weight, tight letter-spacing (-0.02em)
- **Body**: 400-500 weight, standard tracking
- **Labels**: 500 weight, slight positive tracking for uppercase
- **Data**: Monospace, tabular-nums for alignment

**Scale**: 11px, 12px, 13px, 14px (base), 16px, 18px, 24px, 32px

```css
/* Headline */
font-size: 20px;
font-weight: 600;
letter-spacing: -0.02em;
line-height: 1.4;

/* Body */
font-size: 14px;
font-weight: 400;
line-height: 1.5;

/* Data/Code */
font-family: 'SF Mono', monospace;
font-size: 13px;
font-variant-numeric: tabular-nums;
```

---

## Monospace for Data

Numbers, IDs, codes, timestamps belong in monospace. Use `tabular-nums` for columnar alignment. Mono signals "this is data."

---

## Iconography

Use **Phosphor Icons** (`@phosphor-icons/react`). Icons clarify, not decorate — if removing an icon loses no meaning, remove it.

Give standalone icons presence with subtle background containers.

---

## Animation

- 150ms for micro-interactions, 200-250ms for larger transitions
- Easing: `cubic-bezier(0.25, 1, 0.5, 1)` or `ease-out`
- **No spring/bouncy effects** in enterprise UI

```css
/* Good */
transition: all 150ms ease-out;
transition: opacity 100ms ease-out;

/* Bad */
transition: all 500ms ease-in-out;           /* Too slow */
transition: all 200ms cubic-bezier(0.68, -0.55, 0.265, 1.55); /* Bouncy */
```

---

## Contrast Hierarchy

Build a four-level system:

1. **Foreground** (primary) - Main text, important UI
2. **Secondary** - Supporting text, less emphasis
3. **Muted** - Placeholder text, disabled states
4. **Faint** - Borders, dividers, subtle backgrounds

Use all four consistently throughout the interface.

---

## Color for Meaning Only

Gray builds structure. Color only appears when it communicates:
- **Status**: Success (green), Warning (amber), Error (red)
- **Action**: Primary buttons, links
- **Emphasis**: Selected states, focus rings

Decorative color is noise.

```css
/* Structure (gray) */
background: var(--slate-50);
border: 1px solid var(--slate-200);
color: var(--slate-700);

/* Meaning (color) */
background: var(--blue-600);    /* Primary action */
color: var(--green-500);        /* Success state */
border: 2px solid var(--red-500); /* Error emphasis */
```

---

## Isolated Controls

UI controls deserve container treatment. Date pickers, filters, dropdowns — these should feel like crafted objects.

**Never use native form elements for styled UI.** Native `<select>`, `<input type="date">`, and similar elements render OS-native dropdowns that cannot be styled. Build custom components instead:

- Custom select: trigger button + positioned dropdown menu
- Custom date picker: input + calendar popover
- Custom checkbox/radio: styled div with state management

Custom select triggers must use `display: inline-flex` with `white-space: nowrap` to keep text and chevron icons on the same row.

---

## Dark Mode

Dark interfaces have different needs:

**Borders over shadows**
Shadows are less visible on dark backgrounds. Lean more on borders for definition.

**Adjust semantic colors**
Status colors (success, warning, error) often need to be slightly desaturated for dark backgrounds.

**Same structure, different values**
The hierarchy system still applies, just with inverted values.

---

## Anti-Patterns

Avoid these:

- **Dramatic drop shadows**: `box-shadow: 0 10px 50px rgba(0,0,0,0.5)`
- **Border-radius above 16px**: Any radius over 16px (e.g., 24px, 32px) is inappropriate in craft design regardless of element size — the valid range is 4-16px
- **Pure white cards on colored backgrounds**: #fff on #f8f9fa
- **Thick decorative borders**: 4px solid just for decoration
- **Excessive spacing**: >48px margins between elements
- **Decorative gradients**: `background: linear-gradient(purple, pink)`
- **Multiple accent colors**: 3+ accent colors fighting for attention

---

## Design Directions

6 starting points for thinking about product personality:

**Precision & Density**
Tight spacing, monochrome, information-forward. For power users who live in the tool. Think Linear, Raycast.

**Warmth & Approachability**
Generous spacing, soft shadows, friendly colors. For products that want to feel human. Think Notion, Coda.

**Sophistication & Trust**
Cool tones, borders-only or subtle shadows, financial gravitas. For products handling money or sensitive data. Think Stripe, Mercury. **Use borders-only or subtle shadows** — never layered shadows for financial tools.

**Boldness & Clarity**
High contrast, dramatic negative space, confident typography. For products that want to feel modern and decisive.

**Utility & Function**
Muted palette, functional density, clear hierarchy. For products where the work matters more than the chrome. Think GitHub.

**Data & Analysis**
Chart-optimized, technical but accessible, numbers as first-class citizens. For analytics, metrics, business intelligence. **Use borders-only depth** — shadows distract from data.

---

## Self-Check

Before finishing:

1. Did I think about what this product needs, or default?
2. Is my depth strategy consistent throughout?
3. Does every element feel intentional?
4. Are ALL spacing/padding/margin/gap/width/height values multiples of 4px? (Check for 2px, 6px, 10px, 14px violations)
5. Do colors communicate meaning, not decoration?

**Standard**: Looks designed by someone who obsesses over details.

---

For full principles and rationale, query Subcog:
```
subcog_recall(filter: "ns:patterns tag:craft-principles domain:user")
```
