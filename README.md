# Strokeline AI Skills

An installable Agent Skill for generating and editing valid Strokeline scripts.
It teaches an AI to plan diagrams, use the real Strokeline DSL, check syntax and
layout, and return complete editable source.

## Included skill

- `skills/strokeline/SKILL.md` — the core workflow and high-priority rules.
- `skills/strokeline/references/` — exact syntax, diagram recipes, diagnostic recovery, prompt planning, generation checks, and maintenance guidance.
- `skills/strokeline/examples/` — parser-checked scripts illustrating supported patterns.

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

See [the maintenance checklist](skills/strokeline/references/maintenance.md)
before publishing skill updates.

## License

Choose a license before publishing this repository.
