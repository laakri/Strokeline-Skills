# Strokeline AI Skills

An installable Agent Skill for generating and editing valid Strokeline scripts.
It teaches an AI to plan diagrams, use the real Strokeline DSL, check syntax and
layout, and return complete editable source.

## Included skill

- `skills/strokeline/SKILL.md` — the core workflow and high-priority rules.
- `skills/strokeline/references/` — verified syntax, diagnostics, layout,
  timing, visual design, patterns, prompt planning, and quality checks.
- `skills/strokeline/references/bad-good-pairs.md` — common invalid forms and
  corrected Strokeline syntax.
- `skills/strokeline/examples/` — complete parser-checked scripts illustrating
  supported patterns.
- `evals/` — realistic generation prompts and expected output characteristics.

## Install

Copy the `skills/strokeline` directory into the skills directory supported by
your AI coding assistant. Keep the `references` and `examples` directories next
to `SKILL.md`; the skill links to them by relative path. Some assistants discover
project skills from `.github/skills/`, `.claude/skills/`, or another configured
skills directory. Use the location documented by your assistant.

After installation, ask the assistant to create or edit a Strokeline diagram,
whiteboard animation, or vertical video script.

## Compatibility and validation

The skill describes the Strokeline Script DSL, not Mermaid, PlantUML, or a
generic drawing format. Keep it synchronized with the grammar and validator in
the Strokeline application. If that application is available, run scripts
through its parser and validator and resolve every error before delivery. Never
claim a script was executed or validated unless it actually was.

The validator CLI is in the Strokeline app repository. From the app's `web`
directory, validate this sibling checkout with:

```powershell
node scripts\validate.mjs ..\..\Strokeline-Skills\skills\strokeline\examples ..\..\Strokeline-Skills\evals
```

From this repository root, after checking out the app into a sibling
`strokeline` directory, run:

```sh
node strokeline/web/scripts/validate.mjs skills/strokeline/examples evals
```

The GitHub Actions workflow checks out the app's `codex/diagram-authoring`
branch, installs its `web` dependencies, and validates every `.wbs` file under
`examples/` and `evals/`. Warnings are reported for review; any non-`W_`
diagnostic fails validation.

See [the maintenance checklist](skills/strokeline/references/maintenance.md)
before publishing skill updates.

## Repository metadata suggestion

- **Description:** An Agent Skill for planning, writing, and validating
  Strokeline diagram and animation scripts.
- **Topics:** `agent-skills`, `strokeline`, `diagram-as-code`, `whiteboard`,
  `uml`, `flowchart`, `animation`, `ai-assistant`

## License

MIT. See [LICENSE](LICENSE).
