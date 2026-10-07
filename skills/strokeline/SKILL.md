---
name: strokeline
description: Create, repair, and explain valid Strokeline diagram and animation scripts. Use when a user asks for UML, flowcharts, architecture diagrams, whiteboard animations, Reels scripts, or Strokeline DSL help.
---

# Strokeline Script Authoring

Create editable source for Strokeline. Treat the installed Strokeline parser and
validator as the authority; this skill is a generation guide, not a replacement
for the parser.

## Workflow

1. Identify the requested artifact: a still diagram, a multi-scene explanation,
   or a vertical social video. Follow the user's explicit format, audience,
   detail, and style requirements.
2. Choose the smallest layout that communicates the requested idea. For a
   diagram, prefer one compact scene. For a narrated explanation, plan each
   scene around one point and use `SAY` only when narration is wanted.
3. Consult [syntax reference](references/syntax.md) before using less-common
   features; consult [diagram recipes](references/diagram-recipes.md) for UML
   conventions and layout patterns.
   Use [diagnostic recovery](references/diagnostic-playbook.md) when the parser
   reports an error or warning.
   Use [prompt patterns](references/prompt-patterns.md) to turn vague requests
   into structured, visually coherent scripts without inventing requirements.
4. Write only documented keywords and values. Preserve existing keywords
   verbatim. Never convert the task into Mermaid, PlantUML, pseudocode, or
   unsupported DSL.
5. Check block closures, IDs, references, property placement, geometry, text
   fit, arrow direction, and canvas bounds using
   [the validation workflow](references/generation-workflow.md).
6. When the Strokeline application or an official parser is available, compile
   the complete script and fix all errors and relevant warnings. Do not claim
   validation without actually running it.
7. Return the complete corrected script. If the user asks for explanation
   rather than source, explain normally. Follow any output format the user
   explicitly requests.

## Non-negotiable syntax rules

- A script begins with `VERSION 1.0`; set `CANVAS width height` explicitly.
- A scene opens as `SCENE number "Title"` and closes as `END SCENE`.
- Close each object/layout block with `END`; do not add `END` to action
  statements such as `ARROW`, `ANIMATE`, `DELETE`, `WAIT`, `CAMERA`, or `SAY`.
- Create arrow endpoints before their arrows. Use unique IDs within each scene.
- Put each property on its own indented line and use only properties supported
  by the chosen object or statement.
- Use exact enum values and the exact syntax in the references. Do not infer
  syntax from a feature's English name.
- Keep text concise and inside the canvas. Consider social-video safe zones for
  vertical layouts; treat their current dimensions as app-version-specific.
- Avoid overlapping labels, nodes, arrowheads, captions, and controls. Prefer
  clear spacing over adding decorative content.

## Output behavior

For a request to generate a script, output the complete source in the user's
requested format. If they ask for raw source only, provide no fences or prose.
When repairing a script, preserve its intended design and existing keywords;
make the smallest complete correction and check the whole script for repeated
instances of the same error.

## References

- [Syntax reference](references/syntax.md)
- [Generation and validation workflow](references/generation-workflow.md)
- [Diagram recipes](references/diagram-recipes.md)
- [Diagnostic playbook](references/diagnostic-playbook.md)
- [Prompt patterns](references/prompt-patterns.md)
- [Examples](examples/)
