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
4. Run every skill example through the Strokeline parser/compiler. Fix all
   blocking diagnostics; investigate warnings rather than blindly suppressing
   them.
5. Review every documented error code and enum spelling against current source.
6. Search for contradictory older instructions elsewhere in the skill.
7. Update this repository's release notes or compatibility statement.

Do not claim full compatibility with an app release unless the examples and
documented syntax have been checked against that release. If a feature is
uncertain, mark it as version-dependent and avoid presenting guessed syntax as
fact.
