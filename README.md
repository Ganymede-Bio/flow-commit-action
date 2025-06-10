# Flow Commit Action

A GitHub Action for committing flow code changes to Ganymede environments.

## Description

This action automates the process of detecting changes in your Ganymede environment directories and committing those changes to the corresponding Ganymede environment. It handles file detection, base64 encoding, and API communication with Ganymede.

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `environment` | The directory of the environment to commit. Must match the Ganymede environment name. | Yes |
| `ganymede_subdomain` | The Ganymede subdomain where the environment is located. | Yes |
| `ganymede_api_token` | API token for authenticating with Ganymede. | Yes |
| `author_email` | Email of the author for the commit. Required for workflow_dispatch events. | No |

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
          environment: 'my-environment'
          ganymede_subdomain: 'my-subdomain'
          ganymede_api_token: ${{ secrets.GANYMEDE_API_TOKEN }}
          author_email: my-email
```
