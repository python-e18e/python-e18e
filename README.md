# python-e18e

A community effort to make Python projects easier to maintain: remove unnecessary dependencies, improve widely used packages, and help projects adopt modern tooling. Inspired by [e18e](https://e18e.dev/), with a separate repository for each durable concern.

## Repository rollout

The order follows the first repositories created by the [JavaScript e18e organization](https://github.com/e18e): shared issues, module replacements, initiative site, organization profile, then CLI.

| Order | Repository | Purpose | Local checkout |
| --- | --- | --- | --- |
| 1 | [ecosystem-issues](https://github.com/python-e18e/ecosystem-issues) | Coordinate changes across upstream projects | `../ecosystem-issues` |
| 2 | [module-replacements](https://github.com/python-e18e/module-replacements) | Curated package alternatives and evidence | `../module-replacements` |
| 3 | [python-e18e](https://github.com/python-e18e/python-e18e) | Initiative, roadmap, and small docs site | This repository |
| 4 | [.github](https://github.com/python-e18e/.github) | Organization profile | `../.github` |
| 5 | [cli](https://github.com/python-e18e/cli) | Read-only project inventory | `../cli` |
| 6 | [codemods](https://github.com/python-e18e/codemods) | Library and code pattern migrations | `../codemods` |
| 7 | [mcp](https://github.com/python-e18e/mcp) | Agent advice and migration previews | `../mcp` |

Each repository has its own Git history and release path. The [plan](PLAN.md) names later tools and the evidence needed before creating them.

## Codemods and MCP

The tools have independent repositories, Python packages, checks, and CI:

- [codemods](https://github.com/python-e18e/codemods): explicit library import migrations and code pattern upgrades, with preview diffs and optional writes.
- [MCP](https://github.com/python-e18e/mcp): dependency advice, import analysis, migration resources, and codemod previews over stdio, using the separate replacement catalog.

The first recipes cover tomli → tomllib, appdirs → platformdirs, backports.zoneinfo → zoneinfo, and Ruff's lru_cache → cache upgrade. See the [migration rollout](docs/migrations.md) for Python floors, pilot checks, and the coordination queue. Source repositories are available; PyPI releases and volunteer pilots are separate milestones.

```sh
git clone https://github.com/python-e18e/codemods.git
git clone https://github.com/python-e18e/mcp.git
uv run --directory codemods --locked python checks/check_codemods.py
uv run --directory mcp --locked python checks/check_mcp.py
```

## Start here

- Have an improvement for an existing project? Open an issue in `ecosystem-issues` with a reproducible baseline and upstream status.
- Know a package with a better alternative? Add evidence and migration caveats to `module-replacements`.
- Want to inspect a project? Run the `cli` scanner. It can read the separate replacement manifest with `--replacements`.

The [docs landing page](docs/index.html) is static HTML. No site generator or deployment is required for local review.
