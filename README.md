# Maester Action

![Maester Action](https://img.shields.io/badge/GitHub%20Action-Maester-red?style=for-the-badge&logo=github)

Monitor your Microsoft 365 tenant's security configuration using **Maester**, the PowerShell-based test automation framework.

## 🚀 Features

- Run public and private test repositories for Microsoft 365 security configurations.
- Supports **Exchange Online** and **Teams** tests.
- Customizable test runs with include/exclude tags.
- Detailed test results with optional email and Teams notifications.
- Uploads test results as GitHub Action artifacts.
- Supports telemetry control for privacy-conscious workflows.

## 📦 Inputs

| Name                          | Description                                                                                     | Required | Default                     |
|-------------------------------|-------------------------------------------------------------------------------------------------|----------|-----------------------------|
| `tenant_id`                   | Entra ID Tenant ID.                                                                             | ✅       |                             |
| `client_id`                   | App Registration Client ID.                                                                    | ✅       |                             |
| `include_public_tests`        | Include public test repository `maester365/maester-tests` in the test run.                     | ❌       | `true`                      |
| `public_tests_ref`            | The branch or tag of the public tests to use.                                                  | ❌       |                             |
| `include_private_tests`       | Include private tests from the current repository.                                             | ❌       | `true`                      |
| `include_exchange`            | Include Exchange Online tests in the test run.                                                 | ❌       | `false`                     |
| `include_teams`               | Include Teams tests in the test run.                                                           | ❌       | `true`                      |
| `include_tags`                | A list of tags to include in the test run (comma-separated).                                   | ❌       |                             |
| `exclude_tags`                | A list of tags to exclude from the test run (comma-separated).                                 | ❌       |                             |
| `maester_version`             | The version of Maester PowerShell to use (`latest`, `preview`, or specific version).           | ❌       | `latest`                    |
| `pester_verbosity`            | Pester verbosity level (`None`, `Normal`, `Detailed`, `Diagnostic`).                          | ❌       | `None`                      |
| `step_summary`                | Output a summary to GitHub Actions.                                                            | ❌       | `true`                      |
| `artifact_upload`             | Upload test results as GitHub Action artifacts.                                                | ❌       | `true`                      |
| `disable_telemetry`           | Disable telemetry logging.                                                                     | ❌       | `false`                     |
| `mail_recipients`             | A list of email addresses to send the test results to (comma-separated).                      | ❌       |                             |
| `mail_userid`                 | The user ID of the sender of the email.                                                        | ❌       |                             |
| `mail_testresultsuri`         | URI to the detailed test results page.                                                         | ❌       | `${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}` |
| `notification_teams_webhook`  | Webhook URL for sending test results to Teams.                                                 | ❌       |                             |
| `notification_teams_channel_id` | The ID of the Teams channel to send the test results to.                                      | ❌       |                             |
| `notification_teams_team_id`  | The ID of the Teams team to send the test results to.                                           | ❌       |                             |

## 📤 Outputs

| Name            | Description                                      |
|------------------|--------------------------------------------------|
| `results_json`   | The file location of the JSON output of the test results. |

## 🛠️ Usage

Here’s an example of how to use the **Maester Action** in your workflow:

```yaml
name: Run Maester Tests

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run Maester 🧪
        uses: svrooij/maester-action@main
        with:
          tenant_id: ${{ secrets.AZURE_TENANT_ID }}
          client_id: ${{ secrets.AZURE_CLIENT_ID }}
          include_public_tests: true
          include_private_tests: false
          include_exchange: false
          include_teams: true
          maester_version: latest
          disable_telemetry: true
```