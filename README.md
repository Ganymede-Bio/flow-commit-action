# Flow Commit Action

GitHub Action for committing flow code changes to your Ganymede environments.

## Description

This action automates the process of detecting changes in your Ganymede environment directories and committing those changes to the corresponding Ganymede environment. It handles file change detection, base64 encoding, and API communication with Ganymede. If using this action in a push event then the committer email address is used in the API request. This must make an email address of an existing Ganymede user. if using this action in a workflow dispatch than an author_email must be set in the action.

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `environment` | The environment to commit flow code to. This value is shown in the URL. For example, if your Ganymede homepage has the URL https://mycompany.ganymede.bio/mycompany-dev/home, you would use mycompany-dev | Yes |
| `ganymede_subdomain` | The Ganymede subdomain where the environment is located. If your Ganymede URL is mycompany.ganymede.bio, this would be mycompany | Yes |
| `ganymede_api_token` | API token for authenticating with Ganymede. | Yes |
| `author_email` | Email of the author for the commit. Required for workflow_dispatch events. | No |

## Outputs

| Output | Description |
|--------|-------------|
| `files-committed` | List of files committed to Ganymede. |

## Usage

The action in your repo would look like this:

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
