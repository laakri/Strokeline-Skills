# Strokeline DSL syntax reference

This reference follows the app parser, compiler, and validator. The property
matrix below distinguishes what the compiler applies from what it merely
parses. A property marked “parsed, ignored” may still produce no diagnostic:
the grammar’s global property list is not a per-statement type checker.

## Document and scenes

```text
VERSION 1.0
CANVAS 1920 1080
BACKGROUND #FAFAFA
STYLE clean
FONT neat
STROKE 3

SCENE 1 "Short title"
  CREATE title AS TEXT
    POSITION 960 140
    SIZE 54
    TEXT "A clear title"
  END
END SCENE
```

`VERSION 1.0` and `CANVAS width height` are required. The parser accepts
positive custom canvas dimensions; common choices are landscape `1920 1080`,
vertical `1080 1920`, portrait `1080 1350`, and square `1080 1080`. Header
options are `BACKGROUND`, `STYLE`, `FONT`, `STROKE`, `BOARD`, `HAND`, `THEME`,
and `SUBTITLES on|off`. The compiler defaults to a `1920 1080` canvas,
`#FAFAFA` background, stroke width `4`, and handwritten font, but missing
required headers still produce diagnostics.

### Coordinate and size conventions

- `CANVAS width height` is **width first, height second**, in pixels.
  Landscape 16:9 is `CANVAS 1920 1080`; vertical 9:16 is
  `CANVAS 1080 1920`. Never reverse these values to express orientation.
  Sanity-check the inequality before returning: vertical means width < height;
  landscape means width > height. `CANVAS 1920 1080` is landscape, not vertical.
- The origin `(0, 0)` is the upper-left. `x` increases to the right and `y`
  increases downward. `POSITION x y` uses these canvas-pixel coordinates.
- For shapes and text, `POSITION` is the **center**, not the upper-left.
  `WIDTH` and `HEIGHT` extend half their value on each side of that center.
  For example, a `WIDTH 800`, `HEIGHT 220` rectangle at `POSITION 540 900`
  occupies x=140..940 and y=790..1010 on a 1080x1920 canvas.
- A text object's `POSITION` is the center of its laid-out text block.
  `MAXWIDTH` constrains wrapping; it does not move the text or change the
  canvas. Check the rendered block bounds, not only the anchor coordinate.
- Before writing positions, choose the canvas, then keep every visible object's
  full bounds (`center ± half-size`) inside it and inside the safe margins.
  Use a layout grid; do not copy landscape y-coordinates into a vertical
  composition or infer that a bigger canvas automatically fixes clipping.
- With `SUBTITLES on`, reserve the lower-middle subtitle band: the renderer
  centers captions around `y = 0.75 × canvas height`. Do not put essential
  text or diagram nodes behind that band.

For a 1080x1920 vertical video, a conservative starting grid is title centers
around y=250–450, main visuals around y=650–1050, and a takeaway around
y=1120–1250. Leave approximately y=1300–1580 clear when subtitles are on;
then check the actual text/shape bounds and platform UI margins.

Each script needs at least one `SCENE number ["title"]` closed by `END SCENE`.
An optional `TRANSITION fade|wipe|slide|erase|none DURATION time` and
`GAP DURATION time` go after the scene's statements and before `END SCENE`.
Durations use a number followed by `s` or `ms`.

## Statement and property matrix

“Applied” means the compiler maps the property into the document or timeline.
“Parsed, ignored” means syntax accepts the property but the relevant compiler
path does not apply it. “Error” means a malformed or unknown option is
diagnosed; many numeric bounds and references are checked after parsing.

| Statement | Applied properties / operands | Parsed, ignored or restricted | Error cases |
| --- | --- | --- | --- |
| Header | `VERSION`, `CANVAS`, `BACKGROUND`, `STYLE`, `FONT`, `STROKE`, `BOARD`, `HAND`, `THEME`, `SUBTITLES` | Unknown theme/style/board names are not comprehensively rejected; use documented app values. | Missing/unsupported version or canvas; invalid canvas dimensions or `SUBTITLES` value. |
| `CREATE id AS type` and direct shape statements | Shared: `POSITION`, `COLOR`, `STROKE`, `OPACITY`, `DRAW`, `REVEAL`, `PEN`; opt-in `ROUGH`, `ROUGHNESS`, `ROUGHSEED`, `BOWING`, `ROUGHFILL`, `PENFOLLOW`, and shape-specific columns below. | `ROUGHNESS` is 0..4; `ROUGHSEED` is an integer. `ROUGHFILL hachure|cross-hatch|zigzag|dots|solid` uses Rough.js patterns and requires `FILL #hex`; pattern fills appear after the outline completes. Rough sampled draw-on reveals edge groups in order, synchronizing duplicate sketch passes while the pen follows the main pass. `TEXT` on a non-text shape is a centered shape label where that shape renders labels. `LABEL` supplies a label where supported. | Unknown type/property; missing required position/size; nonpositive size/radius; invalid opacity, fit, anchor, gradient/shadow, or other checked range. |
| `INK id` | `POINTS`, `COLOR`, `WIDTH`, `DRAW`, `FREEHAND`, `INKSIZE`, `THINNING`, `SMOOTHING`, `STREAMLINE`, `TAPER`, `PENFOLLOW` | This is the raw coordinate-stroke form: `id` is a name, not a mode. Add `POINTS` with at least two complete `x y` pairs. `RAW` is not a keyword or mode. Freehand controls apply only with `FREEHAND on`; omitted values preserve defaults. | Fewer than two complete points; the name `RAW` without `POINTS`; thinning outside -0.9..0.9; smoothing/streamline outside 0..1. |
| `ARROW from -> to` | `ROUTE`, `VIA`, `HEAD`, `LINESTYLE`, `LABEL`, `SOURCELABEL`, `TARGETLABEL`, `COLOR`, `STROKE`, `PEN`, `DRAW`, `REVEAL` | Other globally known properties are parsed but ignored. | Missing `->`/endpoint; unresolved or later endpoint; invalid route, head, line style, width, or waypoint list. |
| `TABLE id` | `POSITION`, `SIZE width height`, `COLUMNS`, `ROW`, `DIVIDER`, `HEADERCOLOR`, `ALIGN`, `COLOR`, `FILL`, `STROKE`, `PEN`, `OPACITY`, `HIGHLIGHT`, `DRAW`, `REVEAL` | Other globally known properties are parsed but ignored. | Missing columns; row cell count differs from header; invalid divider/highlight/alignment/size. |
| `BARCHART`, `LINECHART`, `PIECHART` | `POSITION`, `SIZE`, repeated `DATA label value` rows. | Other chart properties are not supported. | Unknown chart property, malformed numeric values, or missing `END`. |
| `ANIMATE id verb` | `MOVE`, `SCALE`, `ROTATE`, `OPACITY`, `COLOR`, `FADE`, `HIGHLIGHT`; `DURATION`, `EASE`; `ARC` for `MOVE`; `COLOR` modifier for `COLOR`; highlight selectors `ROW`, `COLUMN`, `CELL`. | `AT` is **parsed, not applied**: it is stored by the parser but not passed into the compiled animation. Modifiers not used by the selected verb do not affect it. | Unknown verb/reference; invalid duration, easing where checked, scale, opacity, color, arc, or highlight selector. |
| `ENTER` / `EXIT` | Target, effect name, optional `DURATION`. | Other trailing tokens are discarded by the parser. | Unknown target/effect; invalid duration. |
| `CAMERA` | `ZOOM`: `TARGET` and/or `SCALE`; `PAN`: `TO x y`; `FOLLOW id`: target; `DRIFT`, `SHAKE`, `RESET`; shared `DURATION`, `EASE`. | Other globally known properties are parsed but ignored. Only the listed camera verbs have runtime behavior. | Unknown reference; invalid nonpositive duration/scale. Unrecognized verbs are not reliably rejected, so do not use them. |
| `SAY "text"` | Required `DURATION`; optional `WHO`, `TONE`, `LANG`, `DETAIL`. | Unknown metadata properties do not change the cue. | Missing quoted text or duration; invalid duration/tone. |
| `WAIT duration` | One duration operand. | Extra operands on the same line are discarded. | Invalid duration. |
| `GROUP [id]`, `PARALLEL [STAGGER duration]` | Group ID or parallel stagger; nested statements. | `GROUP` is organizational, not a visible boundary. | Missing `END`; invalid duration. |
| `STACK id` / `GRID [id]` | `STACK`: `DIRECTION vertical|horizontal`, `GAP n`, optional `AT x y`. `GRID`: positive `COLUMNS n`, `GAP n`, optional `AT x y`. Children are measurable shapes/text. | Layout bounds are calculated from child geometry; connectors and other non-shape statements are not layout children. | Missing required direction/columns/gap, invalid options, unsupported children, or children without measurable bounds. |
| `DEFINE name [PARAMS ...]` / `USE name AS id AT x y [WITH ...]` | Macro body, positional offset, and named argument substitutions. Built-in macros are also available in the compiler. | Macro parameters are plain identifiers; no general expression language is evaluated. | Unclosed definition or unknown macro. |
| `DUPLICATE id FROM source` | Source object plus overrides: `POSITION`, `WIDTH`, `HEIGHT`, `RADIUS`, `COLOR`, `FILL`, `STROKE`, `SIZE`, `TEXT`, `LABEL`. | `GRADIENT` and `SHADOW` are applied only when at least one of `COLOR`, `FILL`, `STROKE`, or `SIZE` is also overridden; otherwise they are parsed but ignored. Other globally known overrides are ignored. | Missing source, duplicate ID. |
| `DELETE id` | Target and optional `DURATION`. | Unknown properties do not affect deletion. | Unknown target or invalid duration. |
| `INK` | `POINTS`; modes `ARROW FROM x y TO x y`, `UNDERLINE id`, `CIRCLE id`; `COLOR`, `WIDTH`, `PEN`, `DRAW`, `REVEAL`, opt-in `FREEHAND on`, `PENFOLLOW on`. | `UNDERLINE` and `CIRCLE` derive geometry from an existing target. `FREEHAND on` uses deterministic variable-width perfect-freehand ink; omitted preserves the legacy ink renderer. | Too few/invalid points, identical arrow points, unknown/unmeasurable target. |

Raw coordinate strokes use a named object and explicit points. Do not write
`INK RAW`; the parser treats `RAW` as an object name, not as a drawing mode.

```text
INK accent_stroke
  POINTS 100 200, 260 180, 420 240
  COLOR #2E86AB
  WIDTH 4
  DRAW 0.8s
END
```

### Shape-specific properties

| Shape type | Applied properties |
| --- | --- |
| `TEXT` | `TEXT`, `SIZE n`, `MAXWIDTH`, `ALIGN`, `LINEHEIGHT`, `FIT WIDTH n HEIGHT n`, `ANCHOR`, text plate `BACKGROUND`, `PADDING`, `CORNERS`, `BOXOPACITY`; shared properties above. `SIZE` defaults to 36. |
| `CIRCLE` | `RADIUS`; optional `TEXT`/`LABEL`, `FILL`, `GRADIENT`, `SHADOW`; shared properties. Radius must be positive. |
| `RECTANGLE` | `WIDTH`, `HEIGHT`, `TEXT`/`LABEL`, `FILL`, `CORNERS`, `GRADIENT`, `SHADOW`; shared properties. Corner radius defaults to 16. |
| `ELLIPSE`, `DIAMOND` | `WIDTH`, `HEIGHT`, `TEXT`/`LABEL`, `FILL`, `GRADIENT`, `SHADOW`; shared properties. |
| `LINE` | `FROM x y`, `TO x y`, `COLOR`, `STROKE`, `PEN`, `LINESTYLE`, `DRAW`, `REVEAL`. Both endpoints are needed for a useful line. |
| `ICON` | `NAME` (or `ICON`), `POSITION`, `SIZE`, `COLOR`, `STROKE`, `OPACITY`, `DRAW`, `REVEAL`; size defaults to 32. |
| `IMAGE` | `URL`, `WIDTH`, `HEIGHT`, `FIT contain|cover`, optional `PADDING`, `CORNERS`, `MASK circle`, `BORDER`; shared position/opacity/reveal properties. URL must be HTTPS or root-relative. **Images require positive `WIDTH` and `HEIGHT`; legacy `SIZE width height` does not apply. `SHADOW` is not supported on images.** |

For `RECTANGLE`, `ELLIPSE`, and `DIAMOND`, legacy `SIZE width height` is
accepted, but prefer explicit `WIDTH` and `HEIGHT`. `CORNERS` applies to
rectangles, not every shape. `GRADIENT` takes two hex colors and `SHADOW` a
blur from 0 to 100; both are supported on circles, ellipses, rectangles, and
diamonds only.

## Rough patterns and smooth connectors

```text
CREATE patterned AS RECTANGLE
  POSITION 260 220
  WIDTH 300
  HEIGHT 160
  FILL #DCECF1
  ROUGH on
  ROUGHNESS 1.5
  ROUGHSEED 17
  ROUGHFILL dots
  DRAW 0.8s
END

ARROW patterned -> destination
  ROUTE elbow
  PENFOLLOW on
  DRAW 0.8s
```

`ROUGHFILL` accepts `hachure`, `cross-hatch`, `zigzag`, `dots`, or `solid`.
Set `FILL #hex` for the pattern color. Patterned fills are rendered when the
outline finishes; `solid` uses a solid Rough.js fill. Rough sampled shape
outlines reveal successive edge groups; paired sketch/overdraw passes reveal
together so one pen can follow the main stroke without retracing a completed
edge. Arrow elbows have rounded corners, and curve routes are sampled smoothly.
The renderer shares that routed geometry between shaft drawing, reveal, and
`PENFOLLOW`.

## Shape example

```text
CREATE decision AS DIAMOND
  POSITION 960 500
  WIDTH 280
  HEIGHT 180
  TEXT "Ready?"
  FILL #EAF2F8
  COLOR #315A72
  STROKE 3
  DRAW 0.6s
END
```

## Connectors

```text
ARROW sourceId -> targetId
  ROUTE elbow
  LINESTYLE dashed
  HEAD open
  LABEL "optional"
  SOURCELABEL "1"
  TARGETLABEL "0..*"
  COLOR #52677A
  STROKE 3
  DRAW 0.5s
```

Create endpoints before arrows. Routes are `straight`, `elbow`, or `curve`;
line styles are `solid`, `dashed`, or `dotted`; heads are `none`, `end`,
`both`, `triangle`, `diamond`, `diamond-filled`, or `open`. Multiplicity text
uses `SOURCELABEL` and `TARGETLABEL`. A self-arrow can represent a sequence
message. Elbow routes have rounded corners; curve routes use smoothly sampled
geometry through their waypoints. Draw-on reveal and `PENFOLLOW` follow that
same route, and the arrowhead appears after the shaft completes. `ARROW` is an
action, not a block: do not add `END`.

## Tables

```text
TABLE account
  POSITION 960 520
  SIZE 640 300
  COLUMNS "Account" "Type"
  ROW "id" "UUID"
  ROW "email" "Text"
  DIVIDER 1
  ROW "+ signIn()" "Method"
  HEADERCOLOR #315A72
  DRAW 0.8s
END
```

Each `ROW` must contain as many cells as `COLUMNS`. Cell text does not wrap;
columns widen to fit content. `DIVIDER n` identifies a body row (starting at
1), and draws the section rule after that row. Tables open a block and require
`END`.

## Animation and camera

```text
ANIMATE card MOVE TO 1400 500 ARC 100
  DURATION 1s
  EASE easeInOut

ANIMATE card OPACITY TO 0.25 DURATION 0.5s
ANIMATE card COLOR TO #E76F51 DURATION 0.5s
ANIMATE account HIGHLIGHT ROW 1 DURATION 0.5s
```

`MOVE TO x y`, `SCALE TO positiveNumber`, `ROTATE TO degrees`,
`OPACITY TO 0..1`, and `COLOR TO #hex` require `TO`. `FADE` and `HIGHLIGHT`
do not. Easing names are `linear`, `easeInOut`, `easeOut`, `easeIn`, `bounce`,
`easeOutBack`, `easeOutElastic`, `easeInOutCubic`, `spring`, and `natural`.
`ARC n` is a `MOVE` modifier in the range -2000..2000. `HIGHLIGHT` can select
table `ROW n`, `COLUMN n`, or `CELL r c`; body rows start at 1, columns include
the header.

`CAMERA ZOOM` accepts `TARGET id` and/or `SCALE n`; `CAMERA PAN` uses
`TO x y`; `CAMERA FOLLOW id` names its target directly. `DRIFT`, `SHAKE`, and
`RESET` are also supported. Camera operations are actions and have no `END`.
Targets must already exist. Reset deliberate camera changes before the scene
ends unless persistent framing is intended.

## Narration and reusable blocks

```text
SAY "One concise spoken sentence."
  DURATION 3s
  TONE explain
  WHO "Narrator"
  LANG en
  DETAIL "Adds context beyond the on-screen text."
```

`SAY` is an action, not a block, and **requires `DURATION`** with an `s` or
`ms` suffix. Valid tones are `explain`, `hook`, `warning`, `punchline`, and
`recap`. `SUBTITLES on|off` sets the initial subtitle state.

`DEFINE name [PARAMS ...]` declares reusable statements and closes with `END`;
`USE name AS id AT x y [WITH key value ...]` instantiates one. `GROUP id` and
`PARALLEL [STAGGER duration]` contain statements and close with `END`. Use a
visible shape for a boundary: `GROUP` is not drawn.

## Block/action distinction

| Block; close with `END` | Action; no `END` |
| --- | --- |
| `CREATE`, `TABLE`, charts, `INK`, `DEFINE`, `GROUP`, `PARALLEL`, `STACK`, `GRID` | `ARROW`, `ANIMATE`, `ENTER`, `EXIT`, `LOOP`, `DELETE`, `DUPLICATE`, `WAIT`, `CAMERA`, `SAY` |
| Scene | Close with `END SCENE` |

When unsure whether a feature works, check the statement/property matrix or
omit it. Do not infer support from a globally recognized property name; confirm
the exact form in the current app parser, compiler, and validator.
