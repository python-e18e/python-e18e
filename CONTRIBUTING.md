# Contributing

Open cross-project opportunities in [ecosystem-issues](https://github.com/python-e18e/ecosystem-issues). Propose package alternatives in [module-replacements](https://github.com/python-e18e/module-replacements). Report scanner bugs and submit code in [cli](https://github.com/python-e18e/cli).

For each proposal, show a real project, supported Python versions, expected benefit, and a check that would catch a changed behavior. Follow the upstream project's contribution guidelines and maintainer decisions.

Submit source migrations in [codemods](https://github.com/python-e18e/codemods) and agent integration changes in [MCP](https://github.com/python-e18e/mcp). Follow the [rollout queue](docs/migrations.md): add only explicit, evidence-backed recipes with version floors, preview diffs, unchanged unsupported forms, and idempotence checks. Reuse Ruff for supported code patterns and the shared codemod function for both entry points. From each checkout, run `uv run --locked python checks/check_codemods.py` or `uv run --locked python checks/check_mcp.py`; record pilot project checks separately before proposing upstream changes.
