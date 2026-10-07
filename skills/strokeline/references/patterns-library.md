# Strokeline patterns library

Use these as complete, small starting scripts. Each uses only app-supported
constructs; adapt IDs, content, canvas, and placement to the request, then run
the complete result through the validator. For longer finished examples, see
[examples/](../examples/).

## Architecture diagram

Use for bounded systems with clear layers. Align each layer and keep enough
space for connector paths.

```text
// Purpose: Show a request path through a small web platform.
// Features used: layered shapes, labels, arrows.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 700
STYLE clean
FONT neat
SCENE 1 "Web platform"
  CREATE browser AS RECTANGLE
    POSITION 220 320
    WIDTH 180
    HEIGHT 100
    TEXT "Browser"
    SIZE 30
  END
  CREATE api AS RECTANGLE
    POSITION 600 320
    WIDTH 180
    HEIGHT 100
    TEXT "API"
    SIZE 30
  END
  CREATE database AS RECTANGLE
    POSITION 980 320
    WIDTH 180
    HEIGHT 100
    TEXT "Database"
    SIZE 30
  END
  ARROW browser -> api
  ARROW api -> database
END SCENE
```

## Sequence flow

Use to explain message order between participants. Keep messages on separate
rows and route self-messages around the participant.

```text
// Purpose: Show a user sign-in request and response.
// Features used: participants, lifelines, directed message arrows.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 800
STYLE clean
FONT neat
SCENE 1 "Sign-in"
  CREATE user AS RECTANGLE
    POSITION 300 120
    WIDTH 180
    HEIGHT 70
    TEXT "User"
    SIZE 28
  END
  CREATE service AS RECTANGLE
    POSITION 900 120
    WIDTH 180
    HEIGHT 70
    TEXT "Service"
    SIZE 28
  END
  CREATE userLife AS LINE
    FROM 300 155
    TO 300 700
    LINESTYLE dashed
  END
  CREATE serviceLife AS LINE
    FROM 900 155
    TO 900 700
    LINESTYLE dashed
  END
  ARROW user -> service
  ARROW service -> user
END SCENE
```

## Use-case diagram

Use for a concise view of user goals within a system boundary. Represent actors
as external text labels unless the app's current icon registry provides a
verified actor symbol; the DSL does not define a dedicated UML actor shape.

```text
// Purpose: Show customer goals inside an online-store system boundary.
// Features used: system boundary, text role, ellipse use cases, association arrows.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none.
VERSION 1.0
CANVAS 1400 800
STYLE clean
FONT neat
BACKGROUND #FAFAFA

SCENE 1 "Online store use cases"
  CREATE boundary AS RECTANGLE
    POSITION 700 410
    WIDTH 860
    HEIGHT 600
    CORNERS 24
    FILL #F4F7FA
    COLOR #455A64
    TEXT "Online store"
    SIZE 34
  END

  CREATE customer AS TEXT
    POSITION 230 400
    SIZE 30
    TEXT "Customer"
    COLOR #315A72
  END

  CREATE browse AS ELLIPSE
    POSITION 470 270
    WIDTH 250
    HEIGHT 100
    FILL #FFFFFF
    COLOR #315A72
    TEXT "Browse products"
    SIZE 28
  END

  CREATE purchase AS ELLIPSE
    POSITION 870 270
    WIDTH 250
    HEIGHT 100
    FILL #FFFFFF
    COLOR #315A72
    TEXT "Place order"
    SIZE 28
  END

  CREATE track AS ELLIPSE
    POSITION 670 540
    WIDTH 250
    HEIGHT 100
    FILL #FFFFFF
    COLOR #315A72
    TEXT "Track delivery"
    SIZE 28
  END

  ARROW customer -> browse
    ROUTE straight
    HEAD none
  ARROW customer -> purchase
    ROUTE elbow
    HEAD none
  ARROW customer -> track
    ROUTE elbow
    HEAD none
END SCENE
```

## State machine

Use for lifecycle states and explicit transitions. Use `DIAMOND` only when the
transition itself branches on a condition.

```text
// Purpose: Show a job moving from queued to running to complete.
// Features used: labeled states and directed transitions.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 700
STYLE clean
FONT neat
SCENE 1 "Job lifecycle"
  CREATE queued AS ELLIPSE
    POSITION 220 350
    WIDTH 190
    HEIGHT 100
    TEXT "Queued"
    SIZE 30
  END
  CREATE running AS ELLIPSE
    POSITION 600 350
    WIDTH 190
    HEIGHT 100
    TEXT "Running"
    SIZE 30
  END
  CREATE complete AS ELLIPSE
    POSITION 980 350
    WIDTH 190
    HEIGHT 100
    TEXT "Complete"
    SIZE 30
  END
  ARROW queued -> running
  ARROW running -> complete
END SCENE
```

## Entity-relationship diagram

Use tables for entities, and endpoint labels for cardinalities. Keep entity
names and row cell counts consistent.

```text
// Purpose: Show customers placing orders.
// Features used: entity tables and multiplicity labels.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 700
STYLE clean
FONT neat
SCENE 1 "Customer orders"
  TABLE customer
    POSITION 320 330
    SIZE 380 220
    COLUMNS "Customer"
    ROW "id: UUID"
    ROW "name: Text"
  END
  TABLE order
    POSITION 880 330
    SIZE 380 220
    COLUMNS "Order"
    ROW "id: UUID"
    ROW "customerId: UUID"
  END
  ARROW customer -> order
    SOURCELABEL "1"
    TARGETLABEL "0..*"
END SCENE
```

## Org chart

Use a top-down hierarchy with short role labels and consistent peer spacing.

```text
// Purpose: Show a small product team reporting structure.
// Features used: hierarchy, repeated rectangle style, arrows.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 800
STYLE clean
FONT neat
SCENE 1 "Product team"
  CREATE lead AS RECTANGLE
    POSITION 600 170
    WIDTH 250
    HEIGHT 100
    TEXT "Product lead"
    SIZE 30
  END
  CREATE design AS RECTANGLE
    POSITION 350 500
    WIDTH 230
    HEIGHT 100
    TEXT "Design"
    SIZE 30
  END
  CREATE engineering AS RECTANGLE
    POSITION 850 500
    WIDTH 260
    HEIGHT 100
    TEXT "Engineering"
    SIZE 30
  END
  ARROW lead -> design
  ARROW lead -> engineering
END SCENE
```

## Timeline

Use aligned milestones for ordered events. Keep dates in separate text objects
so the milestone label remains concise.

```text
// Purpose: Show three delivery milestones.
// Features used: aligned circle milestones and date labels.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1400 750
STYLE clean
FONT neat
SCENE 1 "Delivery plan"
  CREATE plan AS CIRCLE
    POSITION 240 350
    RADIUS 65
    TEXT "Plan"
    SIZE 30
  END
  CREATE build AS CIRCLE
    POSITION 700 350
    RADIUS 65
    TEXT "Build"
    SIZE 30
  END
  CREATE launch AS CIRCLE
    POSITION 1160 350
    RADIUS 65
    TEXT "Launch"
    SIZE 30
  END
  ARROW plan -> build
  ARROW build -> launch
END SCENE
```

## Before/after comparison

Use two side-by-side states for a visual change. Keep the original ID in edit
mode and update only the requested properties.

```text
// Purpose: Compare an unstyled and an improved status card.
// Features used: paired shapes, fill, rounded corners, text labels.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 700
STYLE clean
FONT neat
SCENE 1 "Before and after"
  CREATE before AS RECTANGLE
    POSITION 320 350
    WIDTH 360
    HEIGHT 180
    TEXT "Before"
    SIZE 34
    FILL #E8F1FA
  END
  CREATE after AS RECTANGLE
    POSITION 880 350
    WIDTH 360
    HEIGHT 180
    CORNERS 28
    TEXT "After"
    SIZE 34
    FILL #DDF3EF
  END
END SCENE
```

## Funnel

Use progressively narrower stages to show qualification or conversion. Keep
stage text centered and avoid adding unsupported trapezoid types.

```text
// Purpose: Show a three-step lead funnel.
// Features used: stacked rectangles with decreasing width.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1000 850
STYLE clean
FONT neat
SCENE 1 "Lead funnel"
  CREATE discover AS RECTANGLE
    POSITION 500 200
    WIDTH 600
    HEIGHT 130
    TEXT "Discover"
    SIZE 32
    FILL #E8F1FA
  END
  CREATE qualify AS RECTANGLE
    POSITION 500 410
    WIDTH 460
    HEIGHT 130
    TEXT "Qualify"
    SIZE 32
    FILL #DDE9E7
  END
  CREATE convert AS RECTANGLE
    POSITION 500 620
    WIDTH 320
    HEIGHT 130
    TEXT "Convert"
    SIZE 32
    FILL #FFF3D9
  END
  ARROW discover -> qualify
  ARROW qualify -> convert
END SCENE
```

## Mind map

Use one central idea and a small number of direct branches; add detail only
when it remains readable at the final canvas size.

```text
// Purpose: Map three ways to improve a learning session.
// Features used: central node, radial branches, concise labels.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1200 850
STYLE clean
FONT neat
SCENE 1 "Learning plan"
  CREATE center AS CIRCLE
    POSITION 600 420
    RADIUS 95
    TEXT "Learn"
    SIZE 34
    FILL #DDE9E7
  END
  CREATE read AS RECTANGLE
    POSITION 250 200
    WIDTH 210
    HEIGHT 100
    TEXT "Read"
    SIZE 30
  END
  CREATE practice AS RECTANGLE
    POSITION 950 200
    WIDTH 240
    HEIGHT 100
    TEXT "Practice"
    SIZE 30
  END
  CREATE reflect AS RECTANGLE
    POSITION 600 700
    WIDTH 230
    HEIGHT 100
    TEXT "Reflect"
    SIZE 30
  END
  ARROW center -> read
  ARROW center -> practice
  ARROW center -> reflect
END SCENE
```

## Data flow

Use named stages and directed arrows to distinguish data movement from
ownership or control flow.

```text
// Purpose: Show event data entering a processing pipeline.
// Features used: source, transform, queue, destination.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1400 700
STYLE clean
FONT neat
SCENE 1 "Event data flow"
  CREATE source AS RECTANGLE
    POSITION 200 350
    WIDTH 200
    HEIGHT 110
    TEXT "Producer"
    SIZE 30
  END
  CREATE worker AS RECTANGLE
    POSITION 700 350
    WIDTH 220
    HEIGHT 110
    TEXT "Worker"
    SIZE 30
  END
  CREATE sink AS RECTANGLE
    POSITION 1200 350
    WIDTH 200
    HEIGHT 110
    TEXT "Store"
    SIZE 30
  END
  ARROW source -> worker
  ARROW worker -> sink
END SCENE
```

## Step-by-step tutorial

Use one scene per visible action when the user needs a guided walkthrough.
Avoid advancing the cursor with long waits that do not explain anything.

```text
// Purpose: Teach a three-step save workflow.
// Features used: ordered scenes, short narration, scene transitions.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1080 1920
STYLE clean
FONT neat
SUBTITLES on
SCENE 1 "Edit"
  SAY "First, make the change you want to keep."
    DURATION 3s
    TONE explain
  CREATE step1 AS TEXT
    POSITION 540 720
    SIZE 48
    TEXT "1. Edit the document"
  END
  TRANSITION fade DURATION 0.3s
END SCENE
SCENE 2 "Save"
  SAY "Save it so the change becomes durable."
    DURATION 3s
    TONE explain
  CREATE step2 AS TEXT
    POSITION 540 720
    SIZE 48
    TEXT "2. Save your work"
  END
  TRANSITION fade DURATION 0.3s
END SCENE
SCENE 3 "Confirm"
  SAY "Finally, reopen it and confirm the result."
    DURATION 3s
    TONE recap
  CREATE step3 AS TEXT
    POSITION 540 720
    SIZE 48
    TEXT "3. Confirm it persisted"
  END
END SCENE
```

## Explainer-video storyboard

Use hook → explanation → payoff; each beat should add information instead of
repeating the previous scene.

```text
// Purpose: Explain why backups matter in three storyboard beats.
// Features used: vertical canvas, narration tones, scene transitions.
// Validator status: checked with the app pipeline; zero errors and warnings.
// Remaining warnings: none expected.
VERSION 1.0
CANVAS 1080 1920
THEME notebook
STYLE clean
FONT neat
SUBTITLES on
SCENE 1 "Hook"
  SAY "What happens if your only copy disappears?"
    DURATION 3s
    TONE hook
  CREATE hook AS TEXT
    POSITION 540 650
    SIZE 50
    MAXWIDTH 800
    TEXT "One copy is one point of failure"
  END
  TRANSITION fade DURATION 0.3s
END SCENE
SCENE 2 "Build"
  SAY "A separate backup gives you another path to recovery."
    DURATION 3s
    TONE explain
  CREATE backup AS RECTANGLE
    POSITION 540 900
    WIDTH 680
    HEIGHT 190
    CORNERS 28
    TEXT "Keep a separate recoverable copy"
    SIZE 38
    FILL #E8F1FA
  END
  TRANSITION slide DURATION 0.3s
END SCENE
SCENE 3 "Payoff"
  SAY "Test recovery too, so the backup is useful when needed."
    DURATION 3s
    TONE recap
  CREATE payoff AS TEXT
    POSITION 540 1250
    SIZE 48
    TEXT "Back up. Then test restore."
  END
END SCENE
```

## Architecture callout, org, and ER variants

For specialized architecture contexts, retain the same constraints as the
recipes above: declare nodes before arrows, use tables for structured records,
and do not add syntax by analogy with another diagram language. Use
[sequence-flow.wbs](../examples/sequence-flow.wbs),
[class-diagram.wbs](../examples/class-diagram.wbs), and
[architecture-large.wbs](../examples/architecture-large.wbs) for full verified
larger patterns.
