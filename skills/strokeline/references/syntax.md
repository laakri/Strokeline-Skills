# Strokeline DSL syntax reference

This is a practical subset reference, not a substitute for the current
application grammar. Confirm uncertain syntax against the app's grammar and
validator. Keywords are case-insensitive in normal scripts; preserve the
documented spelling for readability.

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

Canvas presets include landscape `1920 1080`, Reels `1080 1920`, portrait
`1080 1350`, and square `1080 1080`. Custom positive dimensions are supported.

## Shape blocks

Supported `CREATE` types: `TEXT`, `CIRCLE`, `ELLIPSE`, `RECTANGLE`, `DIAMOND`,
`LINE`, `ICON`, and `IMAGE`.

`TEXT` and `ICON` use `SIZE n`; `CIRCLE` uses `RADIUS n`; `RECTANGLE`,
`ELLIPSE`, `DIAMOND`, and `IMAGE` use `WIDTH n` and `HEIGHT n`. Legacy
`SIZE width height` is accepted on dimensional shapes. A missing size is not
automatically inferred for rectangles, ellipses, diamonds, and images.

Common visual properties include `POSITION`, `COLOR`, `STROKE`, `OPACITY`,
`DRAW`, and `REVEAL`, but support varies by type. `FILL` applies to
`CIRCLE`, `ELLIPSE`, `RECTANGLE`, and `DIAMOND`. Do not assume every global
property is valid on every object type.

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

Text objects use `TEXT "..."`, `SIZE n`, and optional `MAXWIDTH n`,
`ALIGN left|center|right`, `LINEHEIGHT n`, `FIT WIDTH n HEIGHT n`,
`ANCHOR center|left|right|top|bottom|topleft|topright|bottomleft|bottomright`,
`BACKGROUND #hex`, `PADDING n`, `CORNERS n`, and `BOXOPACITY 0..1`. `TEXT`
objects need positive `SIZE`; rectangles, diamonds, and ellipses need positive
`WIDTH` and `HEIGHT`; circles need positive `RADIUS`.

Shape labels are centered and auto-fit where supported. Do not assume all
shape types accept `TEXT`; use standalone `TEXT` when uncertain. Avoid manual
line breaks unless the current renderer explicitly supports them.

Filled circles, ellipses, rectangles, and diamonds support
`GRADIENT #top #bottom` and `SHADOW` or `SHADOW blur`. A gradient takes
precedence over `FILL`. `SHADOW` blur ranges from 0 to 100. Rectangles support
`CORNERS n`.

Icons use `CREATE id AS ICON`, then `NAME icon-name` (quoted or unquoted),
`POSITION`, `SIZE`, optional `COLOR`, and `END`. Use only icon names exposed
by the app. Images use
`CREATE id AS IMAGE`, `URL "https://..."` or an official packaged asset,
`WIDTH`, `HEIGHT`, optional `FIT contain|cover`, and `END`.

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

Create both endpoint objects first. Documented routes include `straight`,
`elbow`, and `curve`. Arrow heads include `none`, `end`, `both`, `triangle`,
`diamond`, `diamond-filled`, and `open`. Multiplicity text uses
`SOURCELABEL "..."` and `TARGETLABEL "..."`. Self-arrows can represent sequence
messages.

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

Every row must have the same number of cells as the header. Cell text does not
wrap; use concise values and allow columns to expand. `DIVIDER n` separates
table sections after the indicated body row. For class diagrams, use the table
header for the class name and rows for attributes and operations.

## Layout blocks

`GROUP id`, `PARALLEL`, `STACK id DIRECTION vertical|horizontal GAP n [AT x y]`,
and `GRID [id] COLUMNS n GAP n [AT x y]` are blocks closed with `END`.
`GROUP` organizes related elements; it is not a visible boundary. Use a
rectangle for a visible system or swimlane boundary. `STACK` and `GRID` arrange
measurable children; follow the app's constraints on supported child statements.

An animated group must contain members and be closed before its animation:

```text
GROUP cluster
  CREATE card AS RECTANGLE
    POSITION 400 350
    WIDTH 260
    HEIGHT 130
    TEXT "Worker"
  END
  CREATE label AS TEXT
    POSITION 400 450
    SIZE 28
    TEXT "Processes jobs"
  END
END
ANIMATE cluster MOVE TO 700 350
  DURATION 0.8s
  EASE easeInOut
```

Group animations move, scale, rotate, fade, change opacity, or change color
for member objects together. Groups do not draw a background or boundary.

## Animation and camera

```text
ANIMATE card MOVE TO 1400 500 ARC 100
  DURATION 1s
  EASE easeInOut

ANIMATE card OPACITY TO 0.25
  DURATION 0.5s

ANIMATE card COLOR TO #E76F51
  DURATION 0.5s

ANIMATE card SCALE TO 1.15
  DURATION 0.5s
  EASE easeOut
```

Use `MOVE TO x y`, `SCALE TO positiveNumber`, `ROTATE TO degrees`,
`OPACITY TO 0..1`, and `COLOR TO #hex`. `FADE` and `HIGHLIGHT` do not take
`TO` operands. Table highlights can target `ROW n`, `COLUMN n`, or `CELL r c`;
row and cell numbers count body rows, while column numbers include the header.
`ARC n` is an optional curved-motion operand for `MOVE` and must be between
-2000 and 2000. Add positive `DURATION` and a supported `EASE` on the same
line or following indented lines. Eases include `linear`, `easeIn`,
`easeOut`, `easeInOut`, `bounce`, `easeOutBack`, `easeOutElastic`,
`easeInOutCubic`, `spring`, and `natural`. Avoid `AT` or animation-line
`COLOR` modifiers unless their exact use is confirmed in the app docs; the
verb forms above are the unambiguous basics.

Camera operations are action statements followed by property lines; they do
not use `END`. `CAMERA ZOOM` supports `TARGET id` and/or `SCALE n`;
`CAMERA PAN` uses `TO x y`; `CAMERA FOLLOW id` names its target directly.
`CAMERA DRIFT`, `CAMERA SHAKE`, and `CAMERA RESET` also accept documented
duration/easing settings. Camera target IDs must already exist. Prefer one
deliberate camera change and reset it before the scene ends.

## Narration and transitions

```text
SAY "One concise spoken sentence."
  DURATION 3s
  TONE explain
```

`SAY` is an action, not a block. Its optional following properties include
`DURATION`, `WHO`, `TONE`, `LANG`, and `DETAIL`. Vertical canvases can display
captions for `SAY` cues by default; caption style and visibility can also be
controlled in the app.

After all scene statements, immediately before `END SCENE`, optional
transitions use `TRANSITION fade|wipe|slide|erase|none DURATION time`, followed
by optional `GAP DURATION time`.

## Important block/action distinction

| Opens a block; close with `END` | Action; no `END` |
| --- | --- |
| `CREATE`, `INK`, `TABLE`, charts, `GROUP`, `PARALLEL`, `STACK`, `GRID` | `ARROW`, `ANIMATE`, `DELETE`, `DUPLICATE`, `WAIT`, `CAMERA`, `SAY` |
| Scene block | Close with `END SCENE` |

If a keyword or operand is not covered here, do not improvise. Consult the
current Strokeline app docs or a known-valid script.
