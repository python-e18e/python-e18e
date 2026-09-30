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

## Active next milestone: codemods and MCP

6. **Codemods.** Maintain the independent [codemods repository](https://github.com/python-e18e/codemods) with explicit, reusable library and syntax migrations. Start with tomli, appdirs, zoneinfo backports, and Ruff's cache upgrade rule. Require an explicit minimum Python, preview diffs, formatting preservation, unsupported-case handling, and idempotence checks. The [rollout](docs/migrations.md) names scopes, pilot checks, and release gates.
7. **MCP.** Maintain the independent [MCP repository](https://github.com/python-e18e/mcp) that consumes the explicit replacement manifest and shares codemod previews with the runner. Expose dependency checks, source analysis, recipe listing, guidance resources, and a migration prompt using the official SDK. Source tools take text and return advice or previews; the server does not write target projects.

Both tools have separate source repositories and CI. MCP pins an immutable codemods Git commit so it installs independently. Validate volunteer pilots before PyPI releases; publish codemods before MCP and replace the Git dependency with the released package then. Follow [e18e's ongoing projects](https://e18e.dev/learn/projects) while reusing existing Python tooling. Ruff does not run third-party Flake8 plugins, so contribute missing rules upstream instead of cloning the JavaScript plugin architecture.

## Agent skills

8. **Skills.** Maintain the independent [skills repository](https://github.com/python-e18e/skills), following [e18e/skills](https://github.com/e18e/skills). Distribute a Claude Code marketplace with a Python dependency-replacement skill, an offline catalog snapshot with pinned provenance, and a standard-library matcher. Keep the separate replacement catalog as the source of truth. Skills review usage and compatibility and apply migrations within the user's requested scope; the codemods and MCP remain optional.

## Later work, when evidence supports it

9. **CLI integration and configuration recipes.** Reuse the codemods package after pilots. For limited `setup.cfg` metadata conversion, supported Flake8 settings, and uv's documented import commands, require a real fixture, preview diff, idempotence check, and tests or built-artifact comparison. Keep unsupported settings in place.
10. **framework-tracker / replacement viewer.** Add maintained views only after several projects have active issues and enough data to support them.
11. **setup-publish / action-dependency-diff.** Separate release and CI helpers only after recurring work shows a stable use case.
12. **performance automation.** Benchmark and automate PRs only after measurements, maintainer consent, and rollback checks are routine.

## Migration safety contract

- The CLI is read-only today. A suggestion is a review prompt, not a compatibility guarantee.
- Respect target projects' supported Python versions, dependency markers, build backends, platform data, and custom lint/type rules.
- Before applying a migration to an upstream project, run the relevant project checks. Packaging and configuration migrations also compare wheel and sdist contents, lockfiles, and diagnostics. Preview changes and never delete unsupported configuration silently.
- Prefer uv, Ruff, and ty's documented behavior over reimplementing resolution, linting, or type checking.

## First validation milestone

Try the CLI, catalog, and first codemod previews on volunteer repositories. Record false positives, unsupported configuration, and behavior differences in `ecosystem-issues`. Use pilot results to refine recipe scope before PyPI publication; see the [coordination queue](docs/migrations.md).
