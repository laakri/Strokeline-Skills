# Styling and theming

Use color and type to reinforce structure, never to carry meaning by
themselves. Syntax and exact property support are in
[syntax.md](syntax.md).

## Visual hierarchy

- Set a restrained palette: one canvas/background, one primary ink, one
  emphasis color, and optional semantic colors for distinct states.
- Use one typeface family and two or three sizes. Keep critical labels at
  28px or larger where possible; use bold content and whitespace rather than
  many competing colors.
- Keep stroke weight consistent within a role. Use higher weight only for
  primary paths or boundaries; individual `STROKE` values must be positive.
- Use fills to group or distinguish categories; use `GRADIENT`/`SHADOW`
  sparingly so they do not reduce contrast.
- Check color contrast against the rendered surface. The validator's
  text-contrast threshold is 4.5:1.

## Supported style and font choices

`STYLE` modes implemented by the compiler are `handdrawn`, `chalk`, `marker`,
`pencil`, `brush`, and `clean`. The app maps `FONT handwritten`, `FONT neat`,
`FONT marker`, and `FONT arabic` to installed families. Unknown font names are
not reliably diagnosed; do not invent names.

Use `STYLE clean` and `FONT neat` for technical diagrams and polished
presentations. Use hand-drawn modes intentionally for informal lessons or
storyboards; do not mix pen styles without a clear reason.

## Themes and boards

`THEME name` selects coordinated board, base, ink, and pen defaults. The app's
current theme names are `classic`, `spotlight`, `atlas`, `prism`, `chalk`,
`cosmic`, `foggywindow`, `suspense`, `parchment`, `blueprint`, `cream`,
`deepsea`, `fieldnotes`, `afterhours`, `editorial`, `classroom`, `scrapbook`,
`aurora`, `atelier`, `circuit`, `notebook`, `terrazzo`, `nightwatch`, `pulp`,
`kyoto`, and `gilded`.

`BOARD name` chooses a board surface. Current board names are `chalkboard`,
`whiteboard`, `blueprint`, `foggywindow`, `kraft`, `paper`, `graph`, `dotted`,
`glass`, `plain`, `celestial`, `topographic`, `neon-grid`, `editorial`,
`blackboard`, `corkboard`, `linen`, `aurora`, `circuit`, `notebook`,
`terrazzo`, `sonar`, `halftone`, `zengarden`, `marble`, `spotlight`, `atlas`,
and `prism`.

Use a theme as a starting point, then verify explicit fills and text colors
against the resulting background. Unknown theme/board values may silently fall
back; check names in the current app docs before using them.

## Accessible light/dark variants

- **Light:** warm-white/paper board, dark ink (`#243C46` or similarly dark),
  muted teal/blue accent, and pale fills.
- **Dark:** dark board, near-white ink, a saturated accent for emphasis, and
  restrained mid-value fills. Recheck contrast on every labeled shape.
- Preserve the same semantic color meaning across variants; do not rely on
  red/green alone to distinguish status.
- For accessibility, pair color with words, position, shape, or connector
  pattern. Use dashed/dotted lines only when that distinction is meaningful.
- Preview at the intended output size; readable canvas text can become too
  small after vertical-video scaling.

## Shape paint and text plates

Circles, ellipses, rectangles, and diamonds support `FILL`, two-color
`GRADIENT`, and `SHADOW` blur from 0 to 100. `CORNERS` rounds rectangles;
text-object `BACKGROUND`, `PADDING`, `CORNERS`, and `BOXOPACITY` create a label
plate. These options are type-specific; consult the syntax matrix before
applying them elsewhere.
