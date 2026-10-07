# Pre-delivery quality checklist

Use this list on the complete source, not a partial patch. Run the official
validator CLI when available, fix blocking diagnostics, and inspect warnings
as quality failures until understood. Zero diagnostics do not replace visual
review.

## Source and semantics

- [ ] Starts with `VERSION 1.0` and explicit `CANVAS width height`.
- [ ] Canvas values are width then height (`1080 1920` for 9:16, `1920 1080`
      for 16:9); x grows right, y grows down, and positions use canvas pixels.
- [ ] Has at least one scene; all blocks and scenes close correctly.
- [ ] Every statement/property is documented for that construct in
      [syntax.md](syntax.md); when uncertain, check the matrix or omit it.
- [ ] IDs are unique within each scene; all references point to prior objects.
- [ ] Every table row has the `COLUMNS` cell count; every required shape/image
      has valid dimensions.
- [ ] Arrow direction, multiplicity endpoints, table divider indexes, and
      domain semantics are intentional.
- [ ] Edit mode preserves unrelated IDs, style, ordering, and content.

## Layout and readability

- [ ] Nodes follow a clear grid/reading order; whitespace is reserved for
      labels and connector routes.
- [ ] `POSITION` centers and each object's half-width/half-height bounds fit
      inside the canvas and safe margins; no landscape coordinates were reused
      blindly on a vertical canvas.
- [ ] Text fits without collisions, borders, arrowheads, or unsafe margins.
- [ ] Critical text is preferably 28px or larger; contrast is at least 4.5:1.
- [ ] Vertical composition leaves platform-control and caption space.
- [ ] With `SUBTITLES on`, the lower-middle caption region around
      `0.75 × canvas height` is kept clear of essential content.
- [ ] Palette and pen style remain coherent; color is not the only indicator.

## Timing and motion

- [ ] Reveals/animations support the narrative rather than add decoration.
- [ ] Narration has `DURATION`, stays at four or fewer words per second, and
      adds context instead of echoing visible text.
- [ ] Scene transitions, camera focus, and waits have an intentional purpose.
- [ ] The scene's final camera framing is intentional and reset when appropriate.

## Warning-by-warning quality gate

Resolve every warning or explain why it is intentional:

- [ ] `W_ARROW_CROSSES_TEXT`: reroute arrows or move text so paths avoid words.
- [ ] `W_CAMERA_NOT_RESET`: add `CAMERA RESET` when original framing should be restored.
- [ ] `W_DEAD_AIR`: remove unexplained visual gaps longer than four seconds or add a purposeful change.
- [ ] `W_LONG_TEXT`: add `MAXWIDTH` to text over 60 characters and verify wrapping.
- [ ] `W_LOW_CONTRAST`: raise text/background contrast to at least 4.5:1.
- [ ] `W_REELS_UI_OVERLAP`: keep vertical text below top 125px, above bottom 200px, and left of rightmost 60px; preview the platform overlay.
- [ ] `W_SAY_ECHO`: make narration explain why/how/consequence rather than repeat on-screen words.
- [ ] `W_SAY_FAST`: shorten narration or increase duration to at most four words per second.
- [ ] `W_SAY_OVERLAP`: schedule voice-over as distinct intentional cues.
- [ ] `W_SCENE_LENGTH`: split scenes longer than 60 seconds; for scenes above 20 seconds, ensure meaningful motion is genuinely part of the story.
- [ ] `W_TEXT_OFF_SAFE`: keep text inside x=`width/16`..`width-width/16` and y=`height×100/1080`..`height-height×100/1080`.
- [ ] `W_TEXT_ON_SHAPE`: create backgrounds before text or move text clear of later foreground shapes/icons.
- [ ] `W_TEXT_OVERLAP`: separate simultaneous text so intersection is no more than 8% of the smaller text area.
- [ ] `W_TEXT_TOO_SMALL`: prefer at least 28px; below 18px severity is `error` but the `W_` code is non-blocking, so treat it as a serious readability problem.
- [ ] `W_TOO_CROWDED`: show no more than 12 text objects at once; remove labels or split into scenes.

## Validation and response

- [ ] The complete file was run through the real parser/compiler/validator when
      available; report the exact scope of validation truthfully.
- [ ] New examples report purpose, features used, validator status, and
      remaining warnings in their header comments.
- [ ] All new example `.wbs` files pass with zero blocking errors before commit.
- [ ] Preview was checked at the intended canvas size and animation points.
- [ ] Deliver the complete source in the requested format and add a concise
      summary unless raw-source-only output was requested.

For diagnostic causes and exact repair advice, see
[diagnostics.md](diagnostics.md); for margins and layout techniques, see
[advanced-layout.md](advanced-layout.md).
