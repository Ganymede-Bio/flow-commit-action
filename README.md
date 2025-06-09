# Flow Commit Action

A GitHub Action for committing flow code changes to Ganymede environments.

## Description

This action automates the process of detecting changes in your Ganymede environment directories and committing those changes to the corresponding Ganymede environment. It handles file detection, base64 encoding, and API communication with Ganymede.

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `ENVIRONMENT` | The directory of the environment to commit. Must match the Ganymede environment name. | Yes |
| `GANYMEDE_SUBDOMAIN` | The Ganymede subdomain where the environment is located. | Yes |
| `GANYMEDE_API_TOKEN` | API token for authenticating with Ganymede. | Yes |
| `AUTHOR_EMAIL` | Email of the author for the commit. Required for workflow_dispatch events. | No |

## Outputs

| Output | Description |
|--------|-------------|
| `files-committed` | List of files committed to Ganymede. |

## Usage

```yaml
name: Commit Flow Changes
on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  commit-flow-changes:
    runs-on: ubuntu-latest
    steps:
      - name: Commit flow changes to Ganymede
        uses: ganymede/flow-commit-action@v1
        with:
          ENVIRONMENT: 'my-environment'
          GANYMEDE_SUBDOMAIN: 'my-subdomain'
          GANYMEDE_API_TOKEN: ${{ secrets.GANYMEDE_API_TOKEN }}
          AUTHOR_EMAIL: my-email
```
