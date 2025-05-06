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

| Name             | Description                                      |
|------------------|--------------------------------------------------|
| `results_json`   | The file location of the JSON output of the test results. |
| `tests_total`    | The total number of tests                        |
| `tests_passed`   | Number of passed tests                           |
| `tests_failed`   | Number of failed tests                           |
| `tests_skipped`  | Number of skipped tests                          |
| `result`         | Result of all the tests `Failed` or `Passed`     |


## 🛠️ Usage

Here’s an example of how to use the **Maester Action** in your workflow:

```yaml
name: Run Maester Tests

on:
  push:
    branches:
      - main

  schedule:
    # Daily at 7:30 UTC, change accordingly
    - cron: "30 7 * * *"

  # Allows to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run Maester 🧪
        id: maester
        # Set the action version to a specific version, to keep using that exact version.
        uses: svrooij/maester-action@main
        with:
          tenant_id: ${{ secrets.AZURE_TENANT_ID }}
          client_id: ${{ secrets.AZURE_CLIENT_ID }}
          include_public_tests: true
          include_private_tests: false
          include_exchange: false
          include_teams: true
          # Set a specific version of the powershell module here.
          # check out https://www.powershellgallery.com/packages/Maester/
          maester_version: preview
          disable_telemetry: true
          step_summary: true

      - name: Write status 📃
        shell: bash
        run: |
          echo "The result of the test run is: ${{ steps.maester.outputs.result }}"
          echo "Total tests: ${{ steps.maester.outputs.tests_total }}"
          echo "Passed tests: ${{ steps.maester.outputs.tests_passed }}"
          echo "Failed tests: ${{ steps.maester.outputs.tests_failed }}"
          echo "Skipped tests: ${{ steps.maester.outputs.tests_skipped }}"
```