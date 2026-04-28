# Mike-Jenkins-Org Shared Workflows

Reusable GitHub Actions workflows + org-level meta files (profile README, Copilot instructions, Dependabot config).

> **Note:** All workflows run on **Blacksmith** managed runners (`runs-on: blacksmith`).

## What's in this repo

| Path | Purpose |
|------|---------|
| `profile/README.md` | Org landing page rendered at [github.com/Mike-Jenkins-Org](https://github.com/Mike-Jenkins-Org) |
| `.github/copilot-instructions.md` | PR review guidance for GitHub Copilot on this repo |
| `.github/dependabot.yml` | Weekly Action version bumps for this repo |
| `.github/workflows/lint.yml` | Lints this repo's own markdown + workflow YAML |
| `.github/workflows/security.yml` | Gitleaks scan for this repo |
| `.github/workflows/python-ci.yml` | **Reusable** Python CI for consumer repos |
| `.github/workflows/gitleaks.yml` | **Reusable** gitleaks secret scan for consumer repos |

## Reusable workflows for consumer repos

### Python CI

Standardized Python linting (ruff) and testing (pytest with coverage).

```yaml
# .github/workflows/ci.yml in your repo
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: Mike-Jenkins-Org/.github/.github/workflows/python-ci.yml@main
    with:
      source-dir: src
```

#### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `source-dir` | Yes | — | Source directory to lint and test |
| `python-version` | No | `3.12` | Python version to use |
| `run-mypy` | No | `false` | Run mypy type checking; errors block the build (opt-in) |

### Gitleaks (Secret Scanning)

Detects committed secrets using the gitleaks binary (free, MIT-licensed). The binary is installed at runtime from the latest GitHub release — no Action license required.

```yaml
# .github/workflows/security.yml in your repo
name: Security

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  gitleaks:
    uses: Mike-Jenkins-Org/.github/.github/workflows/gitleaks.yml@main
```

#### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `config-file` | No | (none) | Optional path to a `.gitleaks.toml` config file. If empty, gitleaks defaults are used. |

## Adding new reusable workflows

1. Create the workflow under `.github/workflows/`
2. Use `workflow_call` trigger to make it consumable
3. Set `runs-on: blacksmith`
4. Document inputs in this README
