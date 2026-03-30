# OwlMind Fix CI — GitHub Action

Automatically fix failed CI runs and GitHub issues with AI-powered pull requests.

## Quick Start

### Auto-fix CI failures

```yaml
name: OwlMind Auto-Fix
on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]
jobs:
  fix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: owlmind/fix-ci@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          owlmind-api-key: ${{ secrets.OWLMIND_API_KEY }}
```

### Fix a specific issue

```yaml
name: Fix Issue
on:
  workflow_dispatch:
    inputs:
      issue-url:
        description: 'GitHub issue URL'
        required: true
jobs:
  fix:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: owlmind/fix-ci@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          owlmind-api-key: ${{ secrets.OWLMIND_API_KEY }}
          issue-url: ${{ inputs.issue-url }}
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `github-token` | Yes | — | GitHub token for creating PRs |
| `owlmind-api-key` | Yes | — | OwlMind API key |
| `issue-url` | No | — | Fix a specific issue (if empty, fixes CI failure) |
| `max-attempts` | No | `3` | Maximum fix attempts |
| `base-branch` | No | `main` | Branch to create PR against |

## Outputs

| Output | Description |
|--------|-------------|
| `pr-url` | URL of the created pull request |
| `status` | `fixed`, `failed`, or `skipped` |
| `summary` | Human-readable result summary |

## How it works

1. **CI fails** → OwlMind Action triggers
2. **Reads failure logs** → understands what went wrong
3. **Analyzes codebase** → finds root cause
4. **Writes fix** → creates minimal code change
5. **Runs tests** → verifies fix works
6. **Creates PR** → ready for human review

## Results

- **68% on SWE-bench Lite** (204/300 real GitHub issues solved)
- **7 PRs** in psf/requests (54K★), DRF (30K★), click (17K★), jinja (12K★)
- **$0.02-0.15** per fix

## Get API Key

Sign up at [owlmind.dev](https://owlmind.dev) to get your API key.

## License

Proprietary. See [owlmind.dev](https://owlmind.dev) for pricing.
