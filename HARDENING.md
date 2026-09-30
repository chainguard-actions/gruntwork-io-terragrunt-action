<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string. The offending line is `run: ${{ github.action_path }}/src/main.sh`. Per the check rules, ANY `${{ ... }}` expression directly inside a `run:` block is a script-injection finding — the shell command string is constructed via YAML template substitution before the shell ever sees it. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:83`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ github.action_path }}/src/main.sh` in the `run:` field of the 'Execute Terragrunt' step with `"$GITHUB_ACTION_PATH/src/main.sh"`. GitHub Actions pre-sets the `GITHUB_ACTION_PATH` environment variable to the action's path, so using it directly as a shell variable is both safe and equivalent to the original expression.

