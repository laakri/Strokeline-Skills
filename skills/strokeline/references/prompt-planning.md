# Prompt planning

Translate a natural-language request into a small diagram/storyboard
specification before writing DSL. Separate user-provided facts from visual
choices and illustrative assumptions.

## Diagram specification

| Field | Record |
| --- | --- |
| Goal | What should the viewer understand, choose, or remember? |
| Audience | Who is viewing it, and what do they already know? |
| Artifact | Still diagram, animated explainer, tutorial, or vertical short-form video? |
| Fidelity | Factual model, supplied data, conceptual overview, or clearly marked illustration? |
| Scope | Required entities/steps, relationships, exclusions, and level of detail? |
| Reading order | Left-to-right, top-to-bottom, lifecycle, hierarchy, or hub-and-spoke? |
| Tone/style | Technical, editorial, classroom, playful, branded, or another requested tone? |
| Duration | Required video length or a sensible pacing range? |
| Aspect ratio | Explicit canvas size/preset, or an assumption to state if prose is allowed? |
| Output | Full source, raw source, edited file, explanation, or a diff? |

## Ask versus assume

Ask one concise clarifying question when a missing answer:

- Changes the truth or direction of a factual relationship.
- Changes the set of required entities, steps, or included/excluded scope.
- Determines whether the deliverable is a still diagram or a narrated video.
- Affects brand requirements, accessibility constraints, or an explicit output
  format that cannot be inferred.

Use a conservative, reversible choice without asking when the uncertainty is
only decorative (for example, exact color accent for an unbranded diagram).
For a factual diagram, never invent APIs, dependencies, values, or causal
relationships. If prose is allowed, state an assumption; if source-only output
is required, choose a neutral layout without asserting unsupported facts.

## Creative direction

For creative scenes, start with a visual concept tied to the subject, not a
stock storyboard or a familiar motif chosen by habit. Give each scene a distinct
job and vary scale, composition, or visual action when the idea changes. Keep a
coherent visual throughline, but do not repeat the same layout scene after
scene. Use metaphor to clarify the content, never to add unsupported facts;
decoration should earn its place.

## Planning sequence

1. Rewrite the request as one sentence describing the takeaway.
2. List candidate nodes; remove content that does not support the takeaway.
3. List relationships separately, including direction and meaning.
4. Choose a diagram convention and reading direction; for creative work, use
   patterns only as structural scaffolding, then choose original imagery.
5. Decide whether one scene is sufficient; give each scene one purpose and a
   distinct composition when the story calls for a change of focus.
6. Set numeric canvas width and height in the correct order; sketch horizontal
   and vertical bands and calculate shape bounds from center positions and
   half-sizes before writing coordinates.
7. Reserve whitespace for routes, labels, margins, captions, and platform UI.
   For vertical scripts with subtitles, leave the lower-middle caption band clear.
8. Select only supported shapes/properties from the syntax matrix.
9. Add color and animation only after the unstyled structure is legible.

## Artifact-specific guidance

- **Still:** one scene, no narration or camera moves unless requested; use a
  clear title/focal point and balanced whitespace.
- **Architecture/UML:** preserve domain semantics and arrow direction; ask
  rather than guessing if a relationship is ambiguous.
- **Animation:** plan setup → build → reveal → recap and align `SAY` cues to
  visual beats.
- **Vertical short-form:** use a vertical canvas only when requested or clearly
  appropriate; design for top/right/bottom UI zones and caption space.
- **Edit request:** inventory existing IDs, style, ordering, and working syntax;
  change only the requested feature and return the complete file.

See [patterns-library.md](patterns-library.md) for complete patterns and
[quality-checklist.md](quality-checklist.md) for delivery checks.
