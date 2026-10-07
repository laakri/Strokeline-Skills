---
name: strokeline
description: Create, edit, validate, or explain Strokeline scripts for diagrams, flowcharts, UML, architecture diagrams, sequence diagrams, state machines, ER diagrams, timelines, whiteboard animations, explainer videos, vertical videos, TikTok, Instagram Reels, YouTube Shorts, data flows, org charts, and visual tutorials. Use when users ask for a Strokeline DSL script, animated diagram, or help repairing Strokeline syntax.
---

# Strokeline Script Authoring

Use the installed Strokeline parser, compiler, and validator as the syntax
authority. This skill guides planning and quality; it does not replace the app.

## Workflow

1. **Clarify and plan.** Identify purpose, audience, factual scope, desired
   output, style, duration, and aspect ratio. Ask only when an unknown changes
   correctness or materially affects the result; otherwise make conservative
   visual choices.
2. **Choose a pattern.** Select a suitable diagram/storyboard recipe and decide
   nodes, relationships, reading direction, scene count, and safe margins.
3. **Write the script.** Follow [syntax.md](references/syntax.md). When unsure
   whether a feature works, check the statement/property matrix or omit it.
4. **Self-check.** Run the full source through the app pipeline when available;
   resolve every error and review every warning against
   [diagnostics.md](references/diagnostics.md) and
   [quality-checklist.md](references/quality-checklist.md). Never claim a check
   that was not run.
5. **Return the result.** Provide the complete source in one code block, then a
   2–3 line summary. Honor an explicit raw-source-only or other requested
   output format.

## Never do

- Invent syntax or treat globally recognized properties as valid everywhere.
- Leak Mermaid, PlantUML, pseudocode, or generic drawing syntax into a
  Strokeline script.
- Claim validation, preview, or rendering that did not happen.
- Return partial snippets when the user asks to create or edit a file; return
  the complete source unless they explicitly request a diff/snippet.

## Edit mode

- Preserve existing IDs, visual style, scene ordering, and working syntax.
- Change only what the user asked for; retain unrelated content.
- Check references and layout after edits, and validate the complete result.
- Return the complete edited file, not only changed lines, unless asked
  otherwise.

## Routing

- Exact DSL forms and verified matrix: [syntax.md](references/syntax.md)
- Error causes, repros, and warning rules: [diagnostics.md](references/diagnostics.md)
- Layout and overlap prevention: [advanced-layout.md](references/advanced-layout.md)
- Timing and motion: [animation-and-timing.md](references/animation-and-timing.md)
- Palette and contrast: [styling-and-theming.md](references/styling-and-theming.md)
- Reusable diagram/storyboard patterns: [patterns-library.md](references/patterns-library.md)
- Common invalid/valid pairs: [bad-good-pairs.md](references/bad-good-pairs.md)
- Multi-scene composition and camera: [scene-composition.md](references/scene-composition.md)
- Prompt-to-spec planning: [prompt-planning.md](references/prompt-planning.md)
- Final quality gate: [quality-checklist.md](references/quality-checklist.md)
- Verified complete scripts: [examples/](examples/)
