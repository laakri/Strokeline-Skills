# Strokeline diagnostics

Use the live diagnostic from the app as the final authority. This reference
tracks the current parser, compiler, validator, and script pipeline. Some
codes cover multiple conditions; the examples below show one minimal trigger
for each code family, not every branch.

## Severity and run behavior

The script pipeline blocks when a diagnostic code does not start with `W_`.
Consequently, `W_TEXT_TOO_SMALL` below 18px is displayed with error severity
but is **non-blocking**. Conversely, the current validator reports unsupported
INK highlighting with warning severity under code `E_UNSUPPORTED`; because
that code starts with `E_`, the pipeline currently treats it as blocking.
Treat every `W_` diagnostic as a quality rule even when playback is allowed.

Examples use this common wrapper; insert each repro line or block where
indicated:

```text
VERSION 1.0
CANVAS 800 600
SCENE 1 "Diagnostic"
  // Insert the repro here
END SCENE
```

## Validator/compiler errors

| Code | Cause | Minimal repro inside the scene | Fix |
| --- | --- | --- | --- |
| `E_BAD_RANGE` | A value is outside an accepted range or enum: for example invalid shape opacity, nonpositive dimensions, invalid arrow route/head/line style, malformed gradient, table divider, or image fit. | `CREATE card AS RECTANGLE`<br>`  POSITION 400 300`<br>`  WIDTH 100`<br>`  HEIGHT 80`<br>`  OPACITY 2`<br>`END` | Use the exact range and enum for that property. For opacity use 0–1; for dimensions and durations use positive values; use only documented option names. |
| `E_DUPLICATE_ID` | Two objects in one scene use the same ID, including a duplicate object ID. | `CREATE card AS CIRCLE`<br>`  POSITION 200 200`<br>`  RADIUS 30`<br>`END`<br>`CREATE card AS CIRCLE`<br>`  POSITION 400 200`<br>`  RADIUS 30`<br>`END` | Give each object a unique ID in its scene and update references. |
| `E_IMAGE_URL` | An image URL is missing or is neither HTTPS nor a same-origin root-relative path. | `CREATE logo AS IMAGE`<br>`  POSITION 400 300`<br>`  WIDTH 100`<br>`  HEIGHT 100`<br>`  URL "http://example.com/logo.png"`<br>`END` | Use an accessible `https://...` URL or a root path such as `"/assets/logo.svg"`. |
| `E_MISSING_REQUIRED_PROP` | Required geometry or content is absent: for example shape position/size, table columns, icon name, image dimensions, layout gap, or measurable layout children. | `CREATE card AS RECTANGLE`<br>`  POSITION 400 300`<br>`END` | Supply all required properties for that construct. For this rectangle, add positive `WIDTH` and `HEIGHT`; for an icon use `NAME`. |
| `E_UNKNOWN_ICON` | `ICON`/`NAME` is not in the app's icon registry. | `CREATE mark AS ICON`<br>`  POSITION 400 300`<br>`  SIZE 40`<br>`  NAME not-an-icon`<br>`END` | Choose a name from the app's current icon list; use the validator suggestion when present. |
| `E_UNKNOWN_PROP` | A property is misspelled or not in the global property vocabulary. The parser may accept a property line before validation identifies it. | `CREATE card AS RECTANGLE`<br>`  POSITION 400 300`<br>`  WIDTH 100`<br>`  HEIGHT 80`<br>`  CORNERR 12`<br>`END` | Correct the spelling and verify that the property is supported on this statement in [syntax.md](syntax.md). |
| `E_UNKNOWN_REF` | An arrow, animation, camera, deletion, duplication, ink annotation, or relative placement refers to an object that does not exist yet or is unknown. | `ANIMATE missing MOVE TO 300 200` | Create the target before referencing it; check spelling and scene scope. |
| `E_UNKNOWN_TYPE` | `CREATE` names a type that is not a supported shape. | `CREATE box AS OCTAGON`<br>`  POSITION 400 300`<br>`  WIDTH 100`<br>`  HEIGHT 80`<br>`END` | Use one of `TEXT`, `CIRCLE`, `ELLIPSE`, `RECTANGLE`, `DIAMOND`, `LINE`, `ICON`, or `IMAGE`. |
| `E_UNSUPPORTED` | A layout contains unsupported child statements, or an unsupported operation is requested. For example, layout children must be measurable text/shapes; highlighting INK is not implemented. | `GRID items COLUMNS 1 GAP 20`<br>`  ARROW a -> b`<br>`END` | Keep `STACK`/`GRID` children to measurable shapes/text. For INK highlighting, highlight an eligible shape instead. Note the warning-severity/blocking-code behavior above. |

`E_BAD_RANGE` includes both parser/compiler and validator checks. For a
particular property, follow the live message: it may specify a more precise
bound or required enum than the example row.

## Parser and compiler error index

These codes are emitted before or during document validation. Repair the
construct named by the live message, then rerun the full script.

| Code | Typical cause and repair |
| --- | --- |
| `E_BAD_CANVAS` | `CANVAS` is missing a numeric dimension; provide width and height. |
| `E_BAD_DURATION` | A duration is missing its `s`/`ms` suffix or `SAY` has no duration; use values such as `0.8s` and give every `SAY` a `DURATION`. |
| `E_BAD_EASE` | A reveal easing is unsupported; use `easeOut`, `linear`, or `natural` with `REVEAL`. |
| `E_BAD_RANGE` | A numeric operand or enum is invalid; use the exact accepted range/value described by the diagnostic. |
| `E_BAD_SCENE` | The scene index is not numeric; use `SCENE 1 "Title"`. |
| `E_BAD_SUBTITLES` | `SUBTITLES` is not `on` or `off`. |
| `E_BAD_TONE` | `SAY TONE` is unsupported; use `explain`, `hook`, `warning`, `punchline`, or `recap`. |
| `E_BAD_TRANSITION` | Transition type or required `DURATION` form is invalid; use `fade`, `wipe`, `slide`, `erase`, or `none`. |
| `E_BAD_VERSION` | Version is unsupported; start with `VERSION 1.0`. |
| `E_EXPECTED_ANIMATION` | Animation/effect name is missing; supply the required verb/effect. |
| `E_EXPECTED_ARROW` | Arrow endpoints lack `->`; use `ARROW source -> target`. |
| `E_EXPECTED_CAMERA` | Camera verb is missing; use a supported camera operation. |
| `E_EXPECTED_ID` | An ID is missing; provide a valid object or macro identifier. |
| `E_EXPECTED_NUMBER` | An INK coordinate is missing or nonnumeric; provide complete numeric coordinate pairs. |
| `E_EXPECTED_SCENE` | Top-level content is outside a scene; wrap statements in `SCENE ... END SCENE`. |
| `E_EXPECTED_STRING` | `SAY` text is not quoted; use `SAY "spoken text"`. |
| `E_EXPECTED_TOKEN` | A required structural token such as `AS`, `AT`, `FROM`, or `TO` is missing; match the documented statement form. |
| `E_EXPECTED_TYPE` | `CREATE id AS` has no type; add a supported shape type. |
| `E_INVALID_POINTS` | INK has fewer than two complete points or an arrow whose endpoints are identical; provide at least two distinct `x y` pairs. |
| `E_MISSING_HEADER` | `VERSION 1.0` or `CANVAS width height` is missing or out of order; put both first. |
| `E_MISSING_REQUIRED_PROP` | A table/layout/shape lacks a required property; supply the property named in the message. |
| `E_MISSING_SCENE` | No scene was parsed; add at least one complete scene. |
| `E_PARSER_STALLED` | Parser recovery could not advance after malformed input; inspect and fix the preceding syntax error. |
| `E_SAY_OUTSIDE_SCENE` | `SAY` is in the header/top level; move it inside a scene. |
| `E_TABLE_ROW_LENGTH` | A table row has a different number of cells than `COLUMNS`; make each row match the header count. |
| `E_UNCLOSED_BLOCK` | A block is missing `END` or a scene is missing `END SCENE`; close blocks inside-out. |
| `E_UNEXPECTED_TOKEN` | A token appears where the grammar expects another construct; remove stray text and check block order. |
| `E_UNKNOWN_ANIMATION` | Animation, loop, enter, or exit effect is not implemented; choose a documented effect. |
| `E_UNKNOWN_MACRO` | `USE` names no defined or built-in macro; define it or correct the name. |
| `E_UNKNOWN_PROP` | Unknown property or unsupported layout/chart option; check spelling and the statement-specific matrix. |
| `E_UNKNOWN_REF` | A statement references an unknown or later object; create it first. |
| `E_UNKNOWN_STATEMENT` | A command is not part of the DSL; use a documented Strokeline statement, not Mermaid/PlantUML syntax. |
| `E_UNSUPPORTED` | A layout or operation is not supported; simplify to supported measurable children/operations. |
| `E_SCRIPT_TOO_LARGE` | The source exceeds the app's 56,000-character limit; remove redundant content or split the work. |

## Warning quality rules

Every `W_` code should trigger a visual/timing review, even though the pipeline
does not block solely on a `W_` code.

| Warning | Avoid it by... |
| --- | --- |
| `W_ARROW_CROSSES_TEXT` | Leave a clear corridor between connectors and standalone text. Route around labels with `ROUTE elbow`/`curve` or `VIA`; put important text in a text plate if line crossing is intentional. |
| `W_CAMERA_NOT_RESET` | Add `CAMERA RESET` after the last camera change when the scene should end at its original framing. |
| `W_DEAD_AIR` | Keep visible operations moving or revealing; remove unexplained gaps longer than four seconds, or put a meaningful visual change in that interval. |
| `W_LONG_TEXT` | Give standalone text longer than 60 characters a suitable `MAXWIDTH`, then check its wrapped height and placement. |
| `W_LOW_CONTRAST` | Choose foreground and background colors with at least 4.5:1 contrast; verify text against both the canvas and filled shape/text plate. |
| `W_REELS_UI_OVERLAP` | On vertical canvases, keep text/table-cell bounds below the top 125px, above the bottom 200px, and left of the rightmost 60px; preview against the target platform overlay. |
| `W_SAY_ECHO` | Make narration add a reason, analogy, consequence, or transition instead of repeating visible text. |
| `W_SAY_FAST` | Keep narration at or below four words per second by shortening the cue or increasing `DURATION`. |
| `W_SAY_OVERLAP` | Avoid cues scheduled over the same interval; sequence them deliberately. Playback may auto-schedule overlapping cues, but that is not a substitute for planning the narration beat. |
| `W_SCENE_LENGTH` | Keep a scene at or below 20 seconds unless it includes meaningful move/scale/rotate or camera motion; always split scenes longer than 60 seconds. |
| `W_TEXT_OFF_SAFE` | Keep text bounds within the validator's canvas margin: x between `width/16` and `width-width/16`, y between `height×100/1080` and `height-height×100/1080`. |
| `W_TEXT_ON_SHAPE` | Create background shapes before standalone text so they do not cover it, or move the text clear of later rectangles, circles, ellipses, and icons. |
| `W_TEXT_OVERLAP` | Separate concurrently visible text bounds; keep overlap at or below 8% of the smaller text area. |
| `W_TEXT_TOO_SMALL` | Use at least 28px text where possible. Below 18px the diagnostic severity is `error`, but the `W_` code means it is non-blocking; treat it as a serious readability defect. |
| `W_TOO_CROWDED` | Show no more than 12 text objects at once; split dense explanations across scenes or remove secondary labels. |

## Top 15 mistakes to prevent

1. Omitting `VERSION 1.0` or `CANVAS width height`.
2. Leaving a block or scene without its matching `END` / `END SCENE`.
3. Adding `END` after an action such as `ARROW`, `ANIMATE`, `WAIT`, or `SAY`.
4. Omitting `DURATION` from `SAY`, or writing a duration without `s`/`ms`.
5. Using `SIZE width height` for an image; use `WIDTH` and `HEIGHT`.
6. Giving a table row a different number of cells than its `COLUMNS`.
7. Counting the table header as row 1 for `DIVIDER`; divider indices refer to body rows.
8. Reusing an object ID within a scene or referencing an ID before creation.
9. Assuming a globally recognized property works on every statement; check [syntax.md](syntax.md).
10. Treating parsed-but-ignored syntax as a supported behavior; omit unsupported options.
11. Using `ANIMATE ... AT ...` expecting a delay; `AT` is parsed, not applied.
12. Using an unsupported icon, transition, route, easing, camera verb, or effect name.
13. Placing text where arrows cross it, or letting labels overlap nodes or each other.
14. Shrinking text to fit a crowded canvas instead of simplifying or splitting the scene.
15. Ignoring a warning because playback still runs; inspect every `W_` item, especially safe-zone, contrast, and timing warnings.

## Repair protocol

1. Read the complete diagnostic, including its suggestion and source location.
2. Fix the cause in the source, not in generated scene data.
3. Check for repeated instances of the same mistake elsewhere in the script.
4. Run the complete script through the current parser/compiler/validator.
5. Inspect the rendered result; zero errors does not guarantee legibility.
6. Report only validation that was actually performed, and list any remaining warnings.
