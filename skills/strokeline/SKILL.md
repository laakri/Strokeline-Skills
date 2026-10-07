---
name: strokeline
description: Create, edit, validate, or explain Strokeline scripts for diagrams, flowcharts, UML, architecture diagrams, sequence diagrams, state machines, ER diagrams, timelines, whiteboard animations, explainer videos, vertical videos, TikTok, Instagram Reels, YouTube Shorts, data flows, org charts, and visual tutorials. Use when users ask for a Strokeline DSL script, animated diagram, or help repairing Strokeline syntax.
---

# Strokeline Script Authoring

## Output contract

A Strokeline script is the app's plain-text `.wbs` DSL, not a video brief.
When asked to create one, return executable Strokeline source beginning with
`VERSION 1.0` and `CANVAS width height`, followed by `SCENE` blocks. Use only
statement and property forms documented in [syntax.md](references/syntax.md)
or demonstrated in a checked example. Do not replace the script with YAML,
JSON, a storyboard schema, production metadata, or invented constructs for
camera shots, fonts, audio tracks, keyframes, or effects.

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
   `CANVAS` is `width height`, never `height width`: for vertical 9:16 require
   width < height (usually `1080 1920`); for landscape 16:9 require width >
   height (usually `1920 1080`). Before returning, compare the actual two
   numbers with the requested orientation and fix them if they disagree.
   Calculate placement from center coordinates and object bounds; do not reuse
   positions across aspect ratios.
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
- Return a YAML/JSON video specification or prose storyboard when the user
  asked for a Strokeline script; metadata is not executable `.wbs` source.
- Leak Mermaid, PlantUML, pseudocode, or generic drawing syntax into a
  Strokeline script.
- If the skill/reference cannot be accessed, say so and ask for access or
  source material instead of guessing a substitute language.
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
