# Prompt patterns for better diagrams

Use these internal planning patterns to translate requests into an intentional
diagram. They are not DSL syntax and should not appear in the generated script.

## Parse the request

Extract only information the user gave or that is a safe design choice:

| Dimension | Questions to resolve |
| --- | --- |
| Purpose | What should the viewer understand, decide, or remember? |
| Audience | Beginner, technical peer, executive, student, or general audience? |
| Artifact | Still diagram, multi-scene explanation, or vertical social video? |
| Scope | Which entities, steps, relationships, and exclusions are explicitly requested? |
| Fidelity | Is this conceptual, illustrative, or expected to reflect supplied facts/data? |
| Style | Clean UML, hand-drawn, branded, playful, or another requested visual tone? |
| Output | Raw script, annotated explanation, repair, or example? |

Do not invent factual data, APIs, dependencies, or relationships. For an
illustrative example, label it as an example. If a missing detail changes the
correctness of a factual or domain-specific diagram, ask one short clarifying
question when interaction is allowed. Otherwise state a conservative assumption
only when prose is allowed.

## Choose a visual structure

1. Reduce the request to one sentence describing the takeaway.
2. List the minimum nodes needed to express that takeaway.
3. List relationships separately and assign direction/meaning.
4. Select a reading direction: left-to-right for progression, top-to-bottom
   for hierarchy/sequences, or center-out for one hub with peers.
5. Choose a diagram convention and follow it throughout.
6. Reserve whitespace for connector routes and labels before placing objects.
7. Add visual emphasis only after the structure reads correctly without color.

## Scope by artifact

### Still diagram

- Default to one scene and no narration, waits, mascots, or camera motion.
- Use `STYLE clean`, a plain/light board, and a restrained palette unless the
  user requests another look.
- Include every requested entity/relationship, but avoid decoration that
  competes with the content.

### Animated explanation

- Use one scene per major beat, not one scene per sentence.
- Each beat should add, reveal, transform, or emphasize something visible.
- Let the animation teach a relationship; avoid motion with no explanatory
  purpose.
- Keep narration additive: `SAY` should explain why or what follows rather
  than repeat a visible label.
- Use `PARALLEL` for simultaneous operations only; keep cues and scene timing
  readable.

### Reels/short-form video

- Hook immediately, explain a small number of ideas, then pay off the promise.
- Use short text and short narration cues; large captions consume visual space.
- Keep a single focal point per beat and verify against the app's current safe
  zones.
- Do not shrink important content to fit; simplify or split it instead.

## Layout quality gate

Before returning source, mentally render the diagram at its full canvas:

- Can a viewer identify the title, reading direction, and focal point quickly?
- Is every relationship connected to the correct objects and arrowhead end?
- Are labels readable without zooming and separated from borders/lines?
- Are nodes aligned and consistently sized by role?
- Do connector routes avoid text and unrelated nodes?
- Is whitespace balanced, including the outer canvas margins?
- Does the design still work in grayscale or without shadows?

If any answer is no, revise the layout before adding stylistic detail.

## Minimal repair behavior

When editing a user's script, preserve its scene structure, IDs, keywords,
visual choices, and working syntax unless the user asks to change them. Fix
the root cause first. After syntax repairs, inspect the geometry and timing
that depended on the corrected statement. Return a full script unless the user
asks for a diff or isolated snippet.
