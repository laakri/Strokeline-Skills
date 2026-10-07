# Scene composition

Use multiple scenes when a viewer needs a sequence of distinct ideas or a
deliberate change of focus. A still diagram normally belongs in one scene.

## Multi-scene structure

- Give each scene one purpose and a short optional title.
- Keep IDs unique within each scene; create referenced endpoints before their
  arrows and referenced objects before their animations.
- Repeat a small number of context cues when it helps orientation, but avoid
  rebuilding a dense diagram in every scene.
- Use `TRANSITION fade|wipe|slide|erase|none DURATION time` after scene
  statements and before `END SCENE`. Use `GAP DURATION time` only for an
  intentional pause.
- A transition is a scene-level boundary, not a statement to place between
  individual objects.

```text
SCENE 1 "Input"
  CREATE input AS RECTANGLE
    POSITION 500 350
    WIDTH 300
    HEIGHT 120
    TEXT "Input"
    SIZE 32
  END
  TRANSITION fade DURATION 0.4s
END SCENE

SCENE 2 "Result"
  CREATE result AS RECTANGLE
    POSITION 500 350
    WIDTH 300
    HEIGHT 120
    TEXT "Result"
    SIZE 32
  END
END SCENE
```

## Reusing components

Use `DEFINE name [PARAMS ...] ... END` for a repeated component and
`USE name AS prefix AT x y [WITH key value ...]` to instantiate it. Keep macro
IDs local and pass literal values; the DSL does not evaluate arbitrary
expressions. Built-in macros are implementation-provided, so use them only
when present in the current app references or docs.

`GROUP id ... END` assigns group membership for coordinated transformations.
It does not draw a visible container. Use a separate rectangle when the viewer
needs a visible boundary.

## Camera and focus

- `CAMERA ZOOM` can target an existing object with `TARGET id` and/or set
  `SCALE n`.
- `CAMERA PAN` uses `TO x y`.
- `CAMERA FOLLOW id` follows an existing target.
- `CAMERA DRIFT`, `CAMERA SHAKE`, and `CAMERA RESET` are supported operations.
- Camera operations accept `DURATION` and `EASE`; keep moves deliberate and
  reset at the end if the next scene should start at baseline framing.
- Do not use unrecognized camera verbs: parser acceptance does not guarantee
  runtime behavior.

Camera movement should expose relevant detail, not compensate for an
overcrowded layout. Recheck text size while zoomed and after reset. See
[animation-and-timing.md](animation-and-timing.md) for timing guidance and
[diagnostics.md](diagnostics.md) for the `W_CAMERA_NOT_RESET` rule.
