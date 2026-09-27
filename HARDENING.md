<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a `run:` shell command string. The 'Execute Terragrunt' step uses `run: ${{ github.action_path }}/src/main.sh`, which injects the GitHub Actions expression directly into the shell command before the shell ever sees it. Per the check rules, ANY ${{ ... }} expression inside a run: block is a script-injection finding regardless of which context it reads from.

Locations:

- `action.yml:97`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in the 'Execute Terragrunt' step (action.yml line 97). Moved `${{ github.action_path }}` out of the `run:` shell command and into the step's `env:` block as `ACTION_PATH`. Updated the `run:` command from `${{ github.action_path }}/src/main.sh` to `"$ACTION_PATH/src/main.sh"`, so the expression is no longer directly interpolated into the shell command string.

