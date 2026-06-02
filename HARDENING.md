# Hardening Report: gruntwork-io--terragrunt-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In the 'Execute Terragrunt' step of action.yml, the `github.action_path` context expression is interpolated directly inside the `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. Per the check definition, all `github.*` expressions are considered attacker-controlled and must be passed through an `env:` variable rather than interpolated directly into the run block. The safe pattern is to set `env: ACTION_PATH: ${{ github.action_path }}` and then use `run: "$ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:83`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in the 'Execute Terragrunt' step of action.yml. Moved `${{ github.action_path }}` out of the `run:` shell command string and into the step's `env:` block as `ACTION_PATH: ${{ github.action_path }}`. Updated the `run:` command from `run: ${{ github.action_path }}/src/main.sh` to `run: "$ACTION_PATH/src/main.sh"` so the path is passed via an environment variable rather than being directly interpolated into the shell command.

