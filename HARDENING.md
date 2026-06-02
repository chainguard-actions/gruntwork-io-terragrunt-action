# Hardening Report: gruntwork-io--terragrunt-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Execute Terragrunt' step interpolates `${{ github.action_path }}` directly inside the `run:` value: `run: ${{ github.action_path }}/src/main.sh`. Per the check definition, all `github.*` context values are considered attacker-controlled and must not be interpolated directly in `run:` blocks. The value should be passed via an `env:` variable and referenced as `$ENV_VAR` in the shell command instead.

Locations:

- `action.yml:89`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the 'Execute Terragrunt' step in action.yml (line 89): moved `${{ github.action_path }}` out of the `run:` block and into the `env:` block as `ACTION_PATH: ${{ github.action_path }}`. The `run:` command now uses `"$ACTION_PATH/src/main.sh"` instead of `${{ github.action_path }}/src/main.sh`, preventing direct interpolation of GitHub context expressions in shell commands.

