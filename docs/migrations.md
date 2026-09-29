# Codemods and MCP rollout

The [codemods repository](https://github.com/python-e18e/codemods) owns source edits;
the [MCP repository](https://github.com/python-e18e/mcp) exposes advice and previews.
Both have independent Git history, installable Python packages, checks, and CI.
MCP pins the codemods package to an immutable Git commit. Volunteer pilots and
PyPI publication are the next milestones.

## What we take from JavaScript e18e

| e18e example | Python counterpart | Responsibility |
| --- | --- | --- |
| [module-replacements](https://github.com/e18e/module-replacements) | Existing `module-replacements` repo | Evidence, package alternatives, compatibility caveats. |
| [web-features-codemods](https://github.com/e18e/web-features-codemods) | [codemods repo](https://github.com/python-e18e/codemods) | Reusable, explicit transformations for library imports and code patterns. |
| [CLI](https://github.com/e18e/cli) | Existing `cli` repo | Project discovery and eventual recipe selection. |
| [MCP](https://github.com/e18e/mcp) | [mcp repo](https://github.com/python-e18e/mcp) | Agent tools, migration resources, and a workflow prompt. |
| [ecosystem-issues](https://github.com/e18e/ecosystem-issues) | Existing `ecosystem-issues` repo | Pilot targets, upstream status, and measured outcomes. |

e18e's catalog supplies mappings and guidance; its MCP checks install commands
and source imports, and its codemods provide reusable transformations. Here the
first MCP takes parsed requirement strings, and source previews share the exact
implementation used by the codemod CLI. Existing Ruff rules handle supported
syntax improvements rather than introducing a competing linter.

## First batch

| Migration | Implementation | Required project check |
| --- | --- | --- |
| tomli → tomllib | Preserve bindings while rewriting public imports; Python 3.11+. | Parse representative TOML, exercise errors, confirm no older supported runtime needs tomli. |
| appdirs → platformdirs | Public directory imports and module aliases; compatible platformdirs release, Python 3.10+. | Check storage paths on all supported operating systems and plan existing-data migration. |
| backports.zoneinfo → zoneinfo | Public imports and explicitly aliased module imports; Python 3.9+. | Check `tzdata` availability, packaging, and daylight-saving transitions. |
| lru_cache(maxsize=None) → cache | Delegate to [Ruff UP033](https://docs.astral.sh/ruff/rules/lru-cache-with-maxsize-none/); Python 3.9+. | Review the diff and check caching behavior; unsafe fixes stay disabled. |

The runner requires Python 3.11+; the table describes target code floors. Preview
checks syntax against the declared target and rejects a target newer than the
running interpreter. Import edits are mechanical: they do not inventory every
attribute or prove full API parity. Unsupported cases stay unchanged and are
reported where encountered. File writes require an explicit `--write`.

## Coordination queue

Track each pilot in `ecosystem-issues` with a source URL and revision, recipe ID,
minimum Python, preview diff, required checks, compatibility findings, upstream
status, and measured benefit when available. Maintainers choose whether to
accept a migration. This rollout has not contacted upstream maintainers.

| Order | Deliverable | Location | Acceptance gate | Status |
| --- | --- | --- | --- | --- |
| 1 | Initial recipes and shared preview result | `codemods` repo | Runnable checks for preservation, floors, unsupported forms, and idempotence. | Committed in an independent repository. |
| 2 | stdio agent adapter | `mcp` repo | Real client handshake, four tools, resources, prompt, error cases, and no source execution. | Committed in an independent repository. |
| 3 | Volunteer pilot for each library recipe | `ecosystem-issues` | Review diff; run project checks on minimum Python and relevant OS; record a false-positive or limitation log. | Awaiting pilot targets. |
| 4 | CLI integration | `cli` | Reuse the codemods package; add recipe selection and explicit target floor without duplicating edits. | After pilot feedback. |
| 5 | Independent repositories | `codemods` / `mcp` repos | Independent CI, license, immutable shared dependency, regenerated locks, and standalone installation. | Source repositories created. |
| 6 | PyPI releases | `codemods` / `mcp` repos | Pilot feedback, built artifact checks, release codemods first, replace the MCP Git dependency with a versioned package. | After pilots. |

No pilot success or performance gain is claimed by the synthetic checks.

## Next candidates

Choose the next recipe from recurring pilot findings. Reuse existing migration
tools when their documented scope fits:

- Pydantic v1 → v2: evaluate the upstream
  [migration guide and bump-pydantic](https://docs.pydantic.dev/latest/migration/),
  then check validators, serialization, and public schemas.
- Additional code patterns: select specific
  [Ruff upgrade rules](https://docs.astral.sh/ruff/rules/) with Python floors and
  safe fix behavior, and add one supported recipe at a time.
- requests → httpx, attrs → dataclasses/Pydantic, pytz → zoneinfo, and more-itertools
  replacements: keep catalog advice available, but require call-level and
  behavior fixtures before automating these broader migrations.

Dependency and configuration edits need their own recipes. Inventory all usage
and preserve dependency markers, extras, and lockfile constraints. Configuration
migrations also compare build artifacts and diagnostics as described in
[the organization plan](../PLAN.md).

## Local review

```sh
git clone https://github.com/python-e18e/codemods.git
git clone https://github.com/python-e18e/mcp.git
uv run --directory codemods --locked python checks/check_codemods.py
uv run --directory mcp --locked python checks/check_mcp.py
uv run --project codemods --locked python-e18e-codemod run tomli-to-tomllib /path/to/file.py --target-python 3.11
```

The MCP requires an explicit path to `module-replacements/replacements.json`;
see [its setup](https://github.com/python-e18e/mcp#readme). It loads the catalog at startup and exposes
its original guidance and evidence without copying the catalog into this repo.
