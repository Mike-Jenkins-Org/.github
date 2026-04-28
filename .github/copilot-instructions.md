# Copilot Code Review Instructions

This file guides GitHub Copilot's automated PR reviews for `Mike-Jenkins-Org/.github` — the org's reusable workflows and meta files.

## What to Review

### Security (Critical)

- Hardcoded credentials, tokens, or secrets in workflow YAML
- Workflows that grant excessive permissions (overly broad `permissions:` blocks)
- `pull_request_target` triggers without proper guards (fork-PR exploit risk)
- Use of unpinned third-party actions (floating tags like `@main` or `@v1` where a commit SHA is warranted)
- Reusable workflows that pass secrets to caller workflows without `secrets:` declaration

### Workflow YAML

- Missing `permissions:` block on workflows that need it (defaults can be overly permissive)
- Missing `timeout-minutes` on long-running jobs
- `runs-on:` not set to org standard `blacksmith`
- Reusable workflow inputs without `description:` field
- Reusable workflow inputs without sensible defaults where reasonable
- `continue-on-error: true` used to silence real failures rather than tolerate known flakiness

### Markdown

- Broken internal links between documents
- Outdated workflow references in the consumer-facing README
- Inconsistent code-block fencing or example formatting

## What to Skip

- Whitespace, indentation, or YAML formatting (handled by actionlint in CI)
- Stylistic markdown issues (handled by markdownlint)

## Review Tone

- Focus on substantive issues that affect security, correctness, or consumer-facing clarity
- Be specific — reference the line and suggest a fix
- One comment per issue, not per line
