# Organization plan

## Aim

Follow e18e's sequence: organize useful work first, curate evidence second, explain it publicly third, and automate only the cases that are understood. Keep independent release cycles and issue queues for separate tools.

## Ordered repositories

1. **ecosystem-issues — shared tracker.** Collect concrete opportunities across Python projects. Require a project URL, baseline, proposed change, compatibility checks, and upstream status. Use it to learn which migrations deserve tooling.
2. **module-replacements — data.** Maintain a source-backed manifest of old distributions and alternatives. Start with Python standard-library replacements and explicit version limits. Do not promise drop-in compatibility.
3. **python-e18e — initiative and docs.** Publish the mission, migration guides, contribution routes, and results. The initial site is static HTML; add a generator only when maintaining pages by hand becomes costly.
4. **.github — organization profile.** Point visitors to the three foundational repositories and, later, the CLI. Keep organization-wide metadata here.
5. **cli — analysis and migration entry point.** Begin with a read-only scan for `setup.cfg`, Flake8, pip requirements, and Pyright. Accept the replacement manifest as an explicit input. Stable JSON findings make later integrations possible without coupling the repos.

This order mirrors [e18e's repository history](https://github.com/e18e): `ecosystem-issues`, `module-replacements`, `e18e`, `.github`, then `cli`. Each Python repository has its own Git history.

## Next work, in this order when evidence supports it

6. **Validated migration recipes in `cli`.** Start with a limited `setup.cfg` metadata conversion, supported Flake8 settings, and uv's documented import commands. Each needs a real fixture, preview diff, idempotence check, and tests or built-artifact comparison. Keep unsupported settings in place.
7. **framework-tracker.** Track upstream progress only after several projects have active, maintained issues. Avoid a dashboard without data.
8. **setup-publish / action-dependency-diff.** Separate release and CI helpers only after recurring work shows a stable use case.
9. **codemods / MCP / replacement viewer.** Create independent repos when recipes and consumers exist. Prefer contributing missing Ruff rules upstream: Ruff does not run third-party Flake8 plugins, so an ESLint-plugin-style clone would mislead users.
10. **performance automation.** Benchmark and automate PRs only after measurements, maintainer consent, and rollback checks are routine.

## Migration safety contract

- The CLI is read-only today. A suggestion is a review prompt, not a compatibility guarantee.
- Respect target projects' supported Python versions, dependency markers, build backends, platform data, and custom lint/type rules.
- Before an edit is offered, compare wheel and sdist contents, lockfiles, tests, and diagnostics. Preview changes and never delete unsupported configuration silently.
- Prefer uv, Ruff, and ty's documented behavior over reimplementing resolution, linting, or type checking.

## First validation milestone

Try the CLI and catalog on a few volunteer repositories. Record false positives, unsupported configuration, and the most repeated migration. Use those examples to choose the first safe edit recipe.
