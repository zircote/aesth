---
name: init
description: Initialize aesth skill with Subcog context. Loads design system for current project.
allowed-tools: mcp__plugin_subcog_subcog__subcog_recall, mcp__plugin_subcog_subcog__subcog_capture, Bash, Glob, Grep, Read
---

# aesth init

Initialize aesth skill and load design system from Subcog.

## Pre-Init: Recall User Preferences from Mnemonic

Before initializing, check mnemonic for user's design preferences and prior decisions:

```bash
# Search for design-related user preferences
rg -i "design|aesth|ui|interface|pattern" ~/.claude/mnemonic/ ./.claude/mnemonic/ --glob "*.memory.md" -l | head -10

# Check decisions namespace for design decisions
rg -l "." ~/.claude/mnemonic/*/decisions/ --glob "*.memory.md" 2>/dev/null | head -5
```

Apply recalled preferences:
- User's preferred design personality (Precision, Warmth, etc.)
- Common depth strategy choices
- Spacing preferences across projects
- Color palette preferences

## Process

1. **Query Subcog for Project Design System**:
   ```text
   subcog_recall(filter: "ns:patterns tag:aesth tag:design-direction domain:project")
   ```

2. **Check Results**:
   - **If memories exist**: Load direction, tokens, patterns
   - **If not**: Ready to establish on first UI build

## Output

**If design system exists:**
```text
Aesth initialized.

Design Direction: Precision & Density / Cool Slate / Borders-only
Source: subcog://project/patterns/aesth-direction-a1b2c3d4

Ready to build with established patterns.
Use /aesth:status to see full system.
```

**If no design system:**
```text
Aesth initialized.

No design system found for this project.
Ready to establish when you start building UI.

I'll assess context, suggest direction, and capture patterns automatically.
```

## Workflow After Init

Start building UI. Aesth will:
- Apply existing patterns (if system exists)
- Suggest and establish direction (if new project)
- Capture decisions automatically when you confirm

## Communication

Be invisible. Don't announce internal processes.

**Never say:** "Let me check Subcog..." or "I'm in ESTABLISH MODE"

**Instead:** State suggestions with reasoning and get to work.

## Suggest + Ask

Lead with your recommendation, then confirm:
```text
"This feels like a data-heavy admin tool — I'd go minimal.
Tight spacing, monochrome, borders for depth.
Does that direction feel right?"
```

## After Every Task

Offer to save when you finish building UI:

"Want me to save these patterns to Subcog?"

Always offer — new patterns should be captured whether memories exist or not.
