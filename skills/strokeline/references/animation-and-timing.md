# Animation and timing

Use motion to teach ordering, change, or emphasis. The DSL's timing is
deterministic: ordinary statements advance sequentially; `PARALLEL` runs its
children together, optionally staggered.

## Draw order and reveal

- Declare a visible object before animating or referencing it.
- Create broad backgrounds first, then nodes, then connectors and foreground
  annotations. This also prevents later shapes from covering text.
- `DRAW duration` controls a creation reveal; omit it to use the app's default
  one-second draw-on reveal.
- `REVEAL ease` selects a text-wipe reveal style. Supported reveal eases are
  `easeOut`, `linear`, and `natural`; `natural` maps to spring behavior.
- Use short reveals for secondary objects and reserve slower reveals for the
  main idea. Avoid serially drawing a large architecture if the viewer should
  see its layers together.

## Staggering and parallel beats

```text
PARALLEL STAGGER 0.12s
  CREATE first AS CIRCLE
    POSITION 300 300
    RADIUS 40
  END
  CREATE second AS CIRCLE
    POSITION 500 300
    RADIUS 40
  END
END
```

`PARALLEL` starts all children at the current timeline time; `STAGGER time`
offsets each successive child. Use it to reveal a short series, not to hide
unreadable accumulation. For simultaneous independent objects, omit stagger.

## Animation actions

```text
ANIMATE card MOVE TO 900 420
  DURATION 0.8s
  EASE easeInOut
ANIMATE card SCALE TO 1.05 DURATION 0.4s
ANIMATE card COLOR TO #315A72 DURATION 0.4s
```

Supported animation verbs are `MOVE`, `FADE`, `SCALE`, `ROTATE`, `HIGHLIGHT`,
`OPACITY`, and `COLOR`. Use a positive `DURATION` with `s` or `ms`; easing
names include `linear`, `easeIn`, `easeOut`, `easeInOut`, `bounce`,
`easeOutBack`, `easeOutElastic`, `easeInOutCubic`, `spring`, and `natural`.
`ARC n` is an optional `MOVE` modifier in the range -2000..2000. `AT` on an
`ANIMATE` line is parsed but not applied; do not use it to request a delay.

`ENTER id effect` supports `pop`, `slide-left`, `slide-right`, `slide-up`,
`slide-down`, `fade`, `write`, `drop`, and `zoom`. `EXIT id effect` supports
`fade`, `shrink`, the four slide directions, and `erase`. Both accept optional
`DURATION time`.

`LOOP id effect` supports `float`, `pulse`, `wobble`, `breathe`, and `blink`;
optional parameters are `AMPLITUDE n` and `PERIOD duration`. Prefer restrained
loops and avoid continuous movement that competes with reading.

## Narrative pacing

Structure explainers as **setup → build → reveal → recap**:

1. Setup: establish the question, context, or initial state.
2. Build: add the minimum facts/relationships needed to understand the idea.
3. Reveal: emphasize the key result, comparison, or change.
4. Recap: state the takeaway without repeating every label.

Use one major beat per scene. Keep visual pacing comfortably below the
validator's 20-second still-scene warning threshold; split long scenes rather
than adding motion purely to suppress a warning.

## Voice-over beats

- `SAY` requires `DURATION`; include at most four words per second.
- Align each cue with the visual beat it explains. `SAY` cues in parallel may
  be auto-scheduled by playback, but plan them sequentially for predictable
  voice-over.
- Narration should add explanation, not echo on-screen copy.
- Leave time for reading after a reveal; short captions and concise lines fit
  vertical video better.
- Transitions use `TRANSITION type DURATION time` at the end of a scene.
  `GAP DURATION time` adds a pause after that scene; use only if a pause has a
  clear purpose.

Validate the entire script and inspect the beginning, middle, end, and scene
transitions in preview. Animation diagnostics and repair rules are in
[diagnostics.md](diagnostics.md).
