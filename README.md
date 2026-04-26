# BERB-workflows

Reusable GitHub Actions workflows for the FTNT/BERB project portfolio. **Personal IP.** Public so FTNT-* repos can call into them without storing GitHub tokens (per portfolio secrets policy: 1Password is the only secret manager).

Tier 3 of the four-tier architecture (`BERB-pyproject-template` → `BERB-common` → `BERB-workflows` → pre-commit). See [`FTNT-bdm-portfolio/CONVENTIONS.md`](https://github.com/berb223/FTNT-bdm-portfolio/blob/main/CONVENTIONS.md).

## Workflows

### `python-ci.yml` — uv + ruff + mypy + pytest

Mirrors the inline `ci.yml` that lived in `FTNT-sales-workbench`, `FTNT-bdm-operations`, and `BERB-common` before consolidation. Drop-in replacement.

**Inputs (all optional):**

| Input | Default | Description |
|---|---|---|
| `mypy-path` | `src/` | Path mypy should type-check. Use `src/<package_name>` for single-package repos. |
| `uv-sync-args` | `--all-extras --dev` | Extra args to `uv sync`. |
| `pytest-args` | _(empty)_ | Extra args to `uv run pytest`. |
| `runs-on` | `ubuntu-latest` | Runner image. |

**Usage** (in `.github/workflows/ci.yml` of any consumer repo):

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: berb223/BERB-workflows/.github/workflows/python-ci.yml@v1
    with:
      mypy-path: src/
```

## Versioning

Tags follow semver. Consumers pin to a major-version tag (`@v1`); minor and patch updates flow automatically.

- `v1` — current major, tracks `main`. Floats to the latest `v1.x.y`.
- `v1.0.0`, `v1.0.1`, … — immutable point releases.

Breaking changes go to `v2`.
