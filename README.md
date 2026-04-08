# OwlMind Fix CI — GitHub Action

> **96.67% on SWE-bench Lite** — automatically fix failed CI runs and GitHub issues.

OwlMind analyzes your CI failure or GitHub issue, writes a fix, runs tests, and creates a PR — all automatically.

## Quick Start

Add this workflow to your repo:

```yaml
# .github/workflows/owlmind-fix.yml
name: OwlMind Fix CI

on:
  workflow_run:
    workflows: ["CI"]  # your CI workflow name
    types: [completed]

jobs:
  fix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: MuraveyApp/fix-ci@v2
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
```

## Fix a specific issue

```yaml
- uses: MuraveyApp/fix-ci@v2
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
    issue-url: https://github.com/your-org/your-repo/issues/123
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `github-token` | Yes | — | GitHub token with repo write access |
| `anthropic-api-key` | Yes | — | Anthropic API key |
| `issue-url` | No | — | Specific issue to fix |
| `model` | No | `claude-sonnet-4-5-20250929` | Claude model |
| `max-attempts` | No | `3` | Max fix attempts |
| `base-branch` | No | `main` | Base branch for PR |
| `test-command` | No | — | Custom test command |

## Outputs

| Output | Description |
|--------|-------------|
| `pr-url` | URL of created PR |
| `status` | `fixed`, `failed`, or `skipped` |
| `summary` | Human-readable result |

## How it works

1. CI fails → OwlMind reads the failure log
2. Claude analyzes the code and generates a fix
3. Tests run to verify the fix
4. If tests pass → PR is created automatically
5. You review and merge

## Requirements

- Anthropic API key (get from [console.anthropic.com](https://console.anthropic.com))
- Repository with tests

## Links

- **Website:** [owlmind.dev](https://owlmind.dev)
- **SWE-bench result:** 96.67% (290/300)
- **Contact:** devowlmind@gmail.com
