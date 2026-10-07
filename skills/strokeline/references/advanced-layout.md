# Advanced layout

Use coordinates in canvas pixels. Establish a grid and margins before adding
objects; reserve separate corridors for arrows, labels, titles, and captions.
Exact property forms are in [syntax.md](syntax.md).

## Grid and alignment

- Pick a content rectangle and a consistent spacing unit (for example, 24 or
  32 canvas pixels); use multiples for gutters and node gaps.
- Align peer nodes by center or edge. Keep repeated roles at a consistent size.
- Put hierarchy above children, process stages along one reading axis, and
  feedback loops outside the main flow.
- For repeated shapes, `GRID [id] COLUMNS n GAP n [AT x y] ... END` arranges
  measurable children. `AT` is the layout's upper-left point.
- For one-dimensional layouts, use `STACK id DIRECTION vertical|horizontal
  GAP n [AT x y] ... END`.
- `STACK` and `GRID` children must be measurable shapes/text; do not put arrows,
  tables, chart statements, or arbitrary actions inside these auto-layout
  blocks. They arrange geometry; they are not visible borders.
- `GROUP id ... END` groups elements for coordinated animation, not a visible
  frame. Draw a rectangle separately for a visible boundary.

## Spacing, grouping, and dense diagrams

- Establish minimum node-to-node gaps based on label width and route needs,
  not just shape dimensions. Keep arrowheads and multiplicity labels outside
  node text.
- Create all endpoints before arrows. Route structured edges with
  `ROUTE elbow`; use `VIA x y ...` for an intentional corridor.
- For tables, budget padding inside cells and widen columns for text instead of
  shrinking type. Keep rows concise and ensure every `ROW` matches `COLUMNS`.
- For 30+ node architectures, divide the page into labeled layers; use
  repeated icon nodes where labels would overwhelm the map; connect only
  decision-relevant dependencies. If the architecture is still dense, split
  it into scenes.
- Keep relationship direction and arrowhead ownership consistent. Keep
  sequence lifelines at stable x-coordinates and messages on increasing y
  positions.

## Text and labels

- Keep one short phrase per node. Put explanations in separate text objects
  with controlled `MAXWIDTH`; avoid line-crossings by moving labels before
  rerouting arrows.
- Center shape labels and use adequate inner whitespace. For standalone text,
  use `ALIGN`, `MAXWIDTH`, and optionally a `BACKGROUND` plate with padding.
- Never solve crowded layout by reducing critical text below 28px; remove
  secondary wording or split the content instead.

## Canvas and vertical-video margins

- Read dimensions as **width, then height**: use `CANVAS 1080 1920` for 9:16
  and `CANVAS 1920 1080` for 16:9. The origin is top-left; x grows right and y
  grows down. `POSITION x y` is an object's center, so calculate its edges as
  `x ± WIDTH/2` and `y ± HEIGHT/2` before placing it. Text positions are their
  layout anchor; for centered text, keep the measured text bounds inside the
  safe area too.
- Reserve outer breathing room; the validator's general text-safe bounds are
  `width/16` horizontally and `height×100/1080` vertically. For 1920×1080,
  this is approximately x=120..1800 and y=100..980.
- On vertical canvases, additionally keep important text/table cells below
  y=125, above `height-200`, and left of `width-60`, matching the app's current
  Reels/TikTok/Shorts UI overlap check.
- Build vertical scenes in explicit horizontal bands (title, main visual,
  supporting visual, takeaway) with x centered near `width/2`; derive each y
  from the actual canvas height, not from a landscape template. Check node
  edges, not just centers, against the safe margins.
- When `SUBTITLES on` is set, keep essential diagram content clear of the
  subtitle area centered near `0.75 × canvas height`; on 1080x1920, reserve
  roughly y=1300..1580 before positioning the final content row. A takeaway
  above that band (around y=1120..1250) is usually safer; inspect actual bounds.
- The platform overlay and its zones can change. Preview in the target app and
  do not treat the safe region as a substitute for checking the exported
  composition.
- Keep subtitles/captions separate from essential diagram labels; give the
  lower part of the vertical frame room for the actual caption presentation.

## Prevent every validator warning

Treat all `W_` diagnostics as layout/timing quality rules, even though the app
allows playback. See [diagnostics.md](diagnostics.md) for their source logic.

| Warning | Concrete prevention rule |
| --- | --- |
| `W_ARROW_CROSSES_TEXT` | Keep connector paths out of standalone text bounds; reroute around words or move text to a clear lane. |
| `W_CAMERA_NOT_RESET` | Add `CAMERA RESET` after the last camera change when the intended ending is the original framing. |
| `W_DEAD_AIR` | Do not leave more than four seconds with no visual change; remove long `WAIT`s or place a purposeful reveal/motion in the gap. |
| `W_LONG_TEXT` | Add `MAXWIDTH` to standalone text longer than 60 characters and check the resulting wrapped block still fits. |
| `W_LOW_CONTRAST` | Choose text/background colors with at least 4.5:1 contrast; check text on its actual plate or filled shape. |
| `W_REELS_UI_OVERLAP` | On vertical canvases, keep text below top 125px, above bottom 200px, and left of rightmost 60px; verify with the app overlay. |
| `W_SAY_ECHO` | Make narration add context, cause, analogy, or consequence instead of repeating visible words. |
| `W_SAY_FAST` | Keep cue pace at four or fewer words per second; shorten the line or increase its `DURATION`. |
| `W_SAY_OVERLAP` | Schedule cues as distinct beats rather than relying on automatic rescheduling. |
| `W_SCENE_LENGTH` | Keep scenes to 20 seconds unless meaningful move/scale/rotate or camera motion exists; split scenes over 60 seconds. |
| `W_TEXT_OFF_SAFE` | Keep all visible text inside the validator's general x/y margin formula for the selected canvas. |
| `W_TEXT_ON_SHAPE` | Create backgrounds before text; otherwise move standalone text away from later foreground shapes and icons. |
| `W_TEXT_OVERLAP` | Keep simultaneous text bounds apart so intersection stays at or below 8% of the smaller text area. |
| `W_TEXT_TOO_SMALL` | Prefer 28px or larger. Below 18px the diagnostic severity is `error` but the `W_` code makes it non-blocking; fix it as a serious readability issue. |
| `W_TOO_CROWDED` | Keep no more than 12 text objects visible at once; remove secondary labels or distribute content across scenes. |
