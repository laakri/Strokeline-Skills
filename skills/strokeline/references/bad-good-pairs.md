# Common bad/good Strokeline pairs

Bad snippets are intentionally invalid or misleading and are not expected to
pass validation. Good scripts use confirmed forms; run them through the app
pipeline after adapting them to a project.

## Missing required image dimensions

**Bad — expected `E_MISSING_REQUIRED_PROP`:**

```text
CREATE logo AS IMAGE
  POSITION 400 300
  URL "https://example.com/logo.png"
END
```

**Good — dimensions are required; `SIZE` is not a substitute:**

```text
CREATE logo AS IMAGE
  POSITION 400 300
  WIDTH 120
  HEIGHT 80
  FIT contain
  URL "https://example.com/logo.png"
END
```

## Missing narration duration

**Bad — expected `E_BAD_DURATION`:**

```text
SAY "The system sends a response."
```

**Good:**

```text
SAY "The system sends a response."
  DURATION 2s
```

## Invalid shape property spelling

**Bad — expected `E_UNKNOWN_PROP`:**

```text
CREATE card AS RECTANGLE
  POSITION 400 300
  WIDTH 240
  HEIGHT 120
  CORNERR 16
END
```

**Good:**

```text
CREATE card AS RECTANGLE
  POSITION 400 300
  WIDTH 240
  HEIGHT 120
  CORNERS 16
END
```

## Mermaid/PlantUML leakage

**Bad — this is not Strokeline syntax:**

```text
flowchart TD
  A --> B
```

**Good — create nodes before referencing them:**

```text
VERSION 1.0
CANVAS 800 600
SCENE 1 "Simple flow"
  CREATE start AS RECTANGLE
    POSITION 200 300
    WIDTH 160
    HEIGHT 90
    TEXT "Start"
  END
  CREATE finish AS RECTANGLE
    POSITION 600 300
    WIDTH 160
    HEIGHT 90
    TEXT "Finish"
  END
  ARROW start -> finish
END SCENE
```

## Invented YAML video specification

**Bad — this is metadata, not an executable Strokeline script:**

```yaml
metadata:
  aspect_ratio: "16:9"
scenes:
  - layout: "centered_hero"
    camera:
      movement: "slow_dolly_back"
    visual_elements:
      - animation: "kinetic_reveal_by_word"
```

**Good — use actual Strokeline statements, then add only supported properties:**

```text
VERSION 1.0
CANVAS 1920 1080
SCENE 1 "Opening"
  SAY "A clear idea starts with a useful question."
    DURATION 3s
    TONE hook
  CREATE title AS TEXT
    POSITION 960 420
    SIZE 64
    MAXWIDTH 1400
    ALIGN center
    TEXT "Start with the question"
    DRAW 0.7s
  END
END SCENE
```

## Reversed canvas dimensions

**Bad — this is landscape, despite the vertical-video intent:**

```text
VERSION 1.0
CANVAS 1920 1080
SCENE 1 "Vertical short"
END SCENE
```

**Good — width first, height second; 9:16 is taller than it is wide:**

```text
VERSION 1.0
CANVAS 1080 1920
SCENE 1 "Vertical short"
END SCENE
```
