# Flow Commit Action

GitHub Action for committing flow code changes to your Ganymede environments.

This action is used as part of Ganymede's Self-Managed Repo feature. Self-managed repos allow you to host and manage your Ganymede flow code in your organization, giving you greater control over version control, CI/CD, and code governance. For more information on setting up self-managed repos, please see the [official documentation](https://docs.ganymede.bio/app/configuration/SelfManagedRepo).

## Description

This action automates the process of detecting changes in your Ganymede environment directories and committing those changes to the corresponding Ganymede environment. It handles file change detection, base64 encoding, and API communication with Ganymede. If using this action in a push event then the committer email address is used in the API request. This must make an email address of an existing Ganymede user. if using this action in a workflow dispatch than an author_email must be set in the action.

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `environment` | The environment name to retrieve flow code from. This corresponds to the directory name in your repository (e.g., dev, prod). | Yes |
| `ganymede_environment` | The Ganymede environment to deploy to. This value is shown in the URL. For example, if your Ganymede homepage has the URL https://mycompany.ganymede.bio/mycompany-dev/home, you would use mycompany-dev. Typically configured as a GitHub environment variable. | Yes |
| `ganymede_subdomain` | The Ganymede subdomain where the environment is located. If your Ganymede URL is mycompany.ganymede.bio, this would be mycompany | Yes |
| `ganymede_api_token` | API token for authenticating with Ganymede. | Yes |
| `author_email` | Email of the author for the commit. Required for workflow_dispatch events. | No |

## Outputs

| Output | Description |
|--------|-------------|
| `files-committed` | List of files committed to Ganymede. |

## Usage

For information on setting up the required GitHub variables and secrets (`GANYMEDE_ENVIRONMENT`, `GANYMEDE_SUBDOMAIN`, `GANYMEDE_API_TOKEN`), please see the [Self-Managed Repository documentation](https://docs.ganymede.bio/app/configuration/SelfManagedRepo).

For a complete working example, see the [self-managed-example repository](https://github.com/Ganymede-Bio/self-managed-example).

The action in your repo would look like this:

```yaml
name: Commit Flow Changes
on:
  push:
    branches: [main]
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment to publish flows into'
        required: true
        type: choice
        options:
          - dev
          - prod
      authorEmail:
        description: 'Email of the author for the commit'
        required: true

jobs:
  commit-flow-changes:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment || 'dev' }}
    steps:
      - name: Commit flow changes to Ganymede
        uses: ganymede/flow-commit-action@v1
        with:
          environment: ${{ inputs.environment || 'dev' }}
          ganymede_environment: ${{ vars.GANYMEDE_ENVIRONMENT }}
          ganymede_subdomain: ${{ vars.GANYMEDE_SUBDOMAIN }}
          ganymede_api_token: ${{ secrets.GANYMEDE_API_TOKEN }}
          author_email: ${{ inputs.authorEmail }}
```
