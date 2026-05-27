# Hardening Report: gruntwork-io--terragrunt-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Execute Terragrunt' step in action.yml interpolates the GitHub Actions expression `${{ github.action_path }}` directly inside a `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. Per the check criteria, `github.*` expressions must be assigned to an environment variable via `env:` and then referenced as `$ENV_VAR` in the run step, rather than being interpolated directly into the shell command.

Locations:

- `action.yml:88`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script-injection in action.yml at line 88: moved `${{ github.action_path }}` out of the `run:` shell command string and into the step's `env:` block as `ACTION_PATH: ${{ github.action_path }}`. The `run:` field now uses `"$ACTION_PATH/src/main.sh"` to reference the path as a plain environment variable, preventing direct expression interpolation in the shell command.

