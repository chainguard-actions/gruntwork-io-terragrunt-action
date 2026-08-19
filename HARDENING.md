<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `${{ github.* }}` expression is interpolated directly inside a `run:` shell command string. The step 'Execute Terragrunt' uses `run: ${{ github.action_path }}/src/main.sh`, which injects the `github.action_path` context value directly into the shell command before the shell ever sees it. Any `${{ ... }}` expression in a `run:` block is a script-injection finding regardless of which context it reads from.

Locations:

- `action.yml:72`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in action.yml at line 72 (Execute Terragrunt step). Moved `${{ github.action_path }}` from the `run:` shell command string into the step's `env:` block as `ACTION_PATH: ${{ github.action_path }}`. The `run:` command now uses `"$ACTION_PATH/src/main.sh"` — a plain environment variable reference — instead of directly interpolating the GitHub context expression into the shell command string.

