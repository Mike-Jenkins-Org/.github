# Mike-Jenkins-Org Shared Workflows

Reusable GitHub Actions workflows for the organization.

## Available Workflows

### GitLeaks Secret Scanning

Scans for secrets in your codebase.

```yaml
# .github/workflows/security.yml
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

### Claude Code

Enables @claude mentions on issues and pull requests. Claude will respond to mentions and can help with code reviews, issue triage, and implementation tasks.

```yaml
# .github/workflows/claude.yml
name: Claude

on:
  issue_comment:
    types: [created]
  issues:
    types: [opened, assigned]
  pull_request_review_comment:
    types: [created]
  pull_request_review:
    types: [submitted]

jobs:
  claude:
    uses: Mike-Jenkins-Org/.github/.github/workflows/claude.yml@main
```

Uses Claude OAuth for authentication (no API key required).

### Python CI

Standardized Python linting and testing.

```yaml
# .github/workflows/ci.yml
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
      source-dir: src/your-package
```

#### Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `source-dir` | Yes | - | Source directory to lint and test (e.g., `src/wai`) |
| `python-version` | No | `3.10` | Python version to use |
| `run-mypy` | No | `true` | Whether to run mypy type checking |
| `mypy-strict` | No | `false` | If true, mypy failures block the build |

## Adding New Workflows

1. Create a new workflow in `.github/workflows/`
2. Use `workflow_call` trigger to make it reusable
3. Document inputs in this README
