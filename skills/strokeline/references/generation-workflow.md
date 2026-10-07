# Generation and validation workflow

Use this checklist for new scripts and repairs.

## 1. Plan the canvas

- Select the requested format: landscape, vertical, portrait, or square.
- Decide the number of scenes. Use one scene for a normal still diagram.
- Reserve clear space for titles, nodes, labels, arrows, and any captions.
- For a vertical social video, consider platform controls and use the current
  app's safe-zone overlay. Safe-zone dimensions may change by platform or app
  release; do not hard-code old values into a script.

## 2. Design before writing

- Write down the diagram's core entities, relationships, and reading direction.
- Use UML, ER, sequence, flowchart, or use-case notation consistently.
- Place nodes before arrows and leave gutters for routes and labels.
- Keep labels short. Estimate text width; do not place text over a connector or
  let it touch shape boundaries.
- Use color and decoration to reinforce meaning, not as a substitute for
  structure.

## 3. Write valid structure

- Start with `VERSION 1.0` and one explicit `CANVAS`.
- Number scenes consecutively and close each with `END SCENE`.
- Close every opened object or layout block with one `END`.
- Never put an `END` after action statements unless the docs define a block.
- Use each property only on supported object types.
- Create referenced objects before arrows, animations, highlights, and layout
  relationships that depend on them.
- Avoid duplicate IDs within a scene.
- Declare and close a `GROUP` with member objects before animating its ID.
- Keep actions in timeline order: create an object before animating it, and
  make sure its draw/reveal timing allows the action to be visible.

## 4. Verify geometry and semantics

- Ensure every visible object is inside the canvas, except intentional
  full-bleed backgrounds.
- Check text bounds, node separation, arrow direction, arrowhead ownership,
  multiplicity labels, and route crossings.
- For UML, use standard relationship semantics and directions; see
  [diagram recipes](diagram-recipes.md).
- For vertical scripts, ensure captions do not obscure key content.
- Remove redundant statements rather than hiding errors with guessed keywords.

## 5. Validate with the real app

When the Strokeline application is available:

1. Paste or load the entire script into the app.
2. Run the parser/compiler and inspect all diagnostics.
3. Fix every error; resolve layout warnings when the warning points to content
   the viewer needs to read.
4. Re-run validation after each correction and inspect the preview at the
   requested canvas preset.
5. Check animation at its beginning, middle, and end; seeking should preserve
   layout and camera intent.
6. Say "validated" only if the script was actually checked in the app or by its
   official parser.

If the app is unavailable, perform a static check and be honest about the
validation limit. Do not invent a successful parser result.

For code-based repairs, use the [diagnostics reference](diagnostics.md).
If a feature's syntax is unclear, prefer a known-valid app example over
guessing from its keyword name.

## 6. Return the result

- Return the complete script, not a partial patch, unless the user explicitly
  requests a diff.
- Respect raw-source-only requests.
- For repairs, preserve user intent and existing keyword spellings. Explain
  important assumptions only if prose is allowed.
