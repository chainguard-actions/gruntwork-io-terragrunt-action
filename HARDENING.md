# Hardening Report: gruntwork-io--terragrunt-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

In the 'Execute Terragrunt' step of action.yml, the `run:` field directly interpolates `${{ github.action_path }}` into the shell command string: `run: ${{ github.action_path }}/src/main.sh`. Per the security check definition, all `github.*` expressions are considered attacker-controlled and must not be interpolated directly into `run:` blocks. Instead, the value should be assigned to an environment variable via `env:` and referenced as `$ENV_VAR` in the shell command.

Locations:

- `action.yml:83`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script-injection in the 'Execute Terragrunt' step of action.yml. Moved `${{ github.action_path }}` out of the `run:` field and into the `env:` block as `ACTION_PATH: ${{ github.action_path }}`. The shell command was updated from `run: ${{ github.action_path }}/src/main.sh` to `run: "$ACTION_PATH/src/main.sh"`, referencing the value via the safe environment variable instead of direct expression interpolation.

