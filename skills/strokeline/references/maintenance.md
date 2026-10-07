# Skill maintenance and version sync

The skill must track the Strokeline application, not a remembered or generic
DSL. Before releasing a skill update:

## Sources of truth

In the Strokeline repository, inspect:

- `web/src/dsl/grammar.ts` for shape names, properties, and enum values.
- `web/src/dsl/parser.ts` for exact statement/property syntax and block rules.
- `web/src/dsl/compiler.ts` for accepted forms and semantic compilation.
- `web/src/validator/validate.ts` for ranges, references, and diagnostics.
- `web/src/pages/DocsPage.tsx` for user-facing feature behavior.
- Current parser-valid examples and tests for confirmed use.

The grammar's global property list is not proof that every property is valid
for every object. Check parser dispatch and compiler/validator checks for
type-specific behavior.

## Update procedure

1. Identify the DSL change and the app version/commit it belongs to.
2. Update the syntax reference and affected recipe/diagnostic pages.
3. Add or update at least one example that exercises the syntax.
4. Run every `.wbs` example and eval through the Strokeline parser/compiler/
   validator CLI. Fix all blocking diagnostics and review every warning as a
   quality issue.
5. Review every documented error code and enum spelling against current source.
6. Search for contradictory older instructions elsewhere in the skill.
7. Update this repository's release notes or compatibility statement.

Do not claim full compatibility with an app release unless the examples and
documented syntax have been checked against that release. If a feature is
uncertain, mark it as version-dependent and avoid presenting guessed syntax as
fact.

## Validate scripts

The CLI is maintained in the Strokeline app repository at
`web/scripts/validate.mjs`. From that repository's `web` directory, validate a
skill checkout beside the app with:

```powershell
node scripts\validate.mjs ..\..\Strokeline-Skills\skills\strokeline\examples ..\..\Strokeline-Skills\evals
```

From the skill repository root, when the app has been checked out to a sibling
directory named `strokeline`, run:

```sh
node strokeline/web/scripts/validate.mjs skills/strokeline/examples evals
```

The CLI accepts one or more `.wbs` files or directories and recursively checks
all `.wbs` files. A non-`W_` diagnostic makes the command exit non-zero. `W_`
diagnostics are printed as quality warnings and must still be reviewed; in
particular, `W_TEXT_TOO_SMALL` below 18px has error severity but is non-blocking
because its code starts with `W_`.

## Release checklist

- [ ] Confirm which app branch/commit provides the validator used for release.
- [ ] Audit syntax, property behavior, defaults, diagnostic codes, and examples
      against the app source and user-facing docs.
- [ ] Ensure each example header states purpose, features, validator status,
      and remaining warnings.
- [ ] Run the validator CLI on all example and eval directories; require zero
      blocking diagnostics and explicitly review all warnings.
- [ ] Inspect representative scripts visually in the app; parser success alone
      does not prove legibility or attractive composition.
- [ ] Check for broken relative links, stale instructions, and unintended
      syntax duplication.
- [ ] Update `CHANGELOG.md`, repository compatibility details, and release
      metadata; tag or publish only after all checks pass.
