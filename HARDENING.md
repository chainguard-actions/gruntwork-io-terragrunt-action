<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a `run:` shell command string. In the 'Execute Terragrunt' step, the run: value is `${{ github.action_path }}/src/main.sh`. Although `github.action_path` is not attacker-controlled, any `${{ ... }}` expression directly inside a `run:` block is a script-injection finding — the value flows through YAML template substitution before the shell ever sees it. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:83`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in action.yml line 83 by replacing `${{ github.action_path }}/src/main.sh` with `"$GITHUB_ACTION_PATH/src/main.sh"` in the 'Execute Terragrunt' step's `run:` field. The `$GITHUB_ACTION_PATH` environment variable is pre-set by GitHub Actions and avoids the YAML template substitution that makes `${{ }}` expressions a script-injection risk.

