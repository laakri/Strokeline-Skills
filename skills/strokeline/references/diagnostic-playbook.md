# Diagnostic playbook

Use parser and validator messages as evidence. Do not silence a diagnostic by
deleting required structure, changing the user's intent, or inventing syntax.
Parser errors generally block a script from running; `W_` diagnostics are
warnings and may still render.

## Common blocking errors

| Diagnostic | Typical cause | Repair |
| --- | --- | --- |
| `E_MISSING_HEADER` | Missing `VERSION 1.0` or `CANVAS width height` | Add both at the start, before scenes. |
| `E_BAD_VERSION` | Unsupported version string | Use `VERSION 1.0`. |
| `E_BAD_CANVAS` | Canvas dimensions are absent or malformed | Give two positive numeric dimensions. |
| `E_UNKNOWN_STATEMENT` | Typo, unsupported command, or property written as a statement | Check exact keyword and put properties inside the relevant block. |
| `E_UNKNOWN_PROP` | Misspelled or misplaced property | Check spelling and object-specific support in `syntax.md`. |
| `E_UNCLOSED_BLOCK` | Missing or misplaced `END` / `END SCENE` | Close nested blocks inside-out; action statements do not get `END`. |
| `E_EXPECTED_ARROW` | Connector omitted `->` | Write `ARROW sourceId -> targetId`. |
| `E_DUPLICATE_ID` | An ID is declared more than once in one scene | Rename one object and update every reference to it. |
| `E_UNKNOWN_REF` | A referenced ID is absent or created too late | Create the target first; then write the arrow/action. |
| `E_MISSING_REQUIRED_PROP` | Required geometry is missing | Add a positive radius or width/height appropriate to the shape. |
| `E_BAD_RANGE` | Invalid geometry, opacity, duration, color, route, or selector | Follow the diagnostic's expected range/value and fix the source property. |
| `E_IMAGE_URL` | Invalid or missing image URL | Use HTTPS or a root-relative asset path. |

Exact wording and code availability can change. Prefer the live diagnostic and
its suggested fix over this summary.

## High-value warnings

| Warning | Review |
| --- | --- |
| `W_TEXT_OFF_SAFE` | Move/resize text or table content inside readable canvas margins. |
| `W_TEXT_TOO_SMALL` | Shorten text or allocate more width/height; do not rely on unreadably small type. |
| `W_TEXT_OVERLAP` | Separate overlapping text or simplify the labels. |
| `W_TEXT_ON_SHAPE` | Use the node's built-in label or intentionally adjust the layout. |
| `W_LOW_CONTRAST` | Increase contrast between text and the surface behind it. |
| `W_ARROW_CROSSES_TEXT` | Reroute the arrow or move the text. |
| `W_REELS_UI_OVERLAP` | Move important vertical-video content away from the current platform UI zones; inspect the app overlay. |
| `W_SAY_FAST` | Increase cue duration or shorten narration. |
| `W_SAY_OVERLAP` | Review cue timing; playback may schedule cues automatically. |
| `W_SAY_ECHO` | Make narration add context instead of repeating visible text. |
| `W_SCENE_LENGTH` / `W_DEAD_AIR` | Align visual timing with narration and remove unexplained idle time. |
| `W_CAMERA_NOT_RESET` | Reset the camera before scene end unless persistent framing is intentional. |

Warnings are contextual. Do not blindly “fix” an intentional overlap if the
result is legible, but inspect it in preview.

## Repair protocol

1. Read the full diagnostic, source line, and suggested fix.
2. Identify the smallest structural or semantic cause.
3. Repair source, not generated IR or the diagnostic message.
4. Re-run the complete script through the parser/compiler.
5. Check that the repair did not introduce errors, alter unrelated keywords,
   or damage geometry/meaning.
6. Review remaining warnings individually and report validation status
   accurately.
