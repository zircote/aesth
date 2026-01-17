---
name: aesth:init
description: Initialize aesth skill with Subcog context. Loads design system for current project.
---

# aesth init

Initialize aesth skill and load design system from Subcog.

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

Design Direction: [Personality] / [Foundation] / [Depth]
Source: subcog://project/patterns/{urn}

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
