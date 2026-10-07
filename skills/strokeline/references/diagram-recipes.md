# Strokeline diagram recipes

Prefer a single clean scene for a requested diagram. Use the output's canvas
dimensions consistently; positions are canvas-space coordinates.

## Class diagram

- Use one `TABLE` per class.
- Put the class name in the first header cell; list attributes and operations
  as rows.
- Place `DIVIDER n` after the last attribute row where supported.
- Limit columns and row lengths so the class stays legible at the full canvas
  preview size.
- Inheritance: subclass to superclass, `HEAD triangle`.
- Aggregation: hollow `HEAD diamond` at the whole.
- Composition: filled `HEAD diamond-filled` at the whole.
- Dependency: dashed line with `HEAD open`.
- Create every class table before writing its `ARROW` statements.
- Keep relationship labels and arrowheads away from table text.

## Use-case diagram

- Put actors outside the system boundary; use a visible rectangle for the
  boundary and ellipses for use cases.
- Connect actors only to relevant use cases.
- Include relationship: arrow from the including use case to the required
  included use case, dashed with `HEAD open`, label `«include»`.
- Extend relationship: arrow from the optional extending use case back to the
  base use case, dashed with `HEAD open`, label `«extend»`.
- Make the system-boundary rectangle large enough to contain all use cases;
  actors stay outside it.
- Avoid routing dependency arrows through use-case ellipses.

## Sequence diagram

- Arrange participants evenly along the top.
- Draw dashed vertical lifelines.
- Use descending message rows; use an arrow per message and reverse it for a
  response.
- Keep messages between adjacent participants where possible.
- Use self-arrows for a participant's message to itself.
- Put concise message labels near the line without collision.
- Reuse participant x-coordinates for each lifeline and message endpoint;
  increase y consistently for each later message.
- Inspect explicit-coordinate lifelines and messages in preview before adding
  more participants; dense crossings are hard to read on a sequence diagram.

## Flowchart

- Use ellipses for start and end, rounded rectangles for actions, diamonds for
  decisions.
- Label each decision branch and keep branch directions consistent.
- Use elbow routes or waypoints to route around nodes.
- Keep a simple top-to-bottom or left-to-right reading order.
- Label both decision outcomes (for example, "yes" and "no") and keep each
  branch's direction consistent.
- Avoid crossed branches; reroute around nodes with an elbow or waypoints.

## Entity relationship diagram

- Use one `TABLE` per entity; indicate primary/foreign keys in field names.
- Label relationships with cardinalities such as `1` and `0..*`.
- Ensure connector arrow direction follows the intended relationship.
- Avoid crossing connectors and place cardinalities next to the correct ends.

## Vertical short-form video

- Use a vertical canvas preset such as `1080 1920`.
- Build a clear hook, a small number of content beats, then a concise payoff.
- Use short `SAY` cues; preview the captions and choose a style in the app.
- Keep essential text away from currently shown platform UI zones. The overlay
  is a guide, not an exported graphic.
- Do not overfill the frame. One idea per scene is easier to read on a phone.

## Layout heuristics

- Use `STACK` for one-dimensional groups and `GRID` for repeated peer nodes.
- Use a visible rectangle for lanes and system boundaries; `GROUP` is not a
  visible frame.
- Draw broad containers first, nodes second, connectors after endpoints, then
  labels and annotations.
- Use elbow routes for structured systems, curves for feedback, and straight
  arrows only when the direct path is clear.
- Add one visual emphasis at a time. Maintain consistent node sizes and
  spacing.
