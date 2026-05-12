# Hardening Report: gruntwork-io--terragrunt-action/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in action.yml pin to the mutable tag `@v2` instead of a full 40-character SHA commit hash. This exposes the action to supply-chain attacks if the upstream tag is moved. Failing references: `jdx/mise-action@v2` (appears twice, for the 'Install tools with mise (using mise.toml)' and 'Install tools with mise (using input versions)' steps).

Locations:

- `action.yml:63`
- `action.yml:70`

### script-injection (severity: high)

The 'Execute Terragrunt' step uses `run: ${{ github.action_path }}/src/main.sh`, which directly interpolates a `github.*` context expression inside a `run:` shell command. Per the check rules, `github.*` expressions must be assigned to an environment variable via `env:` and referenced as `$ENV_VAR` in the run step, rather than being interpolated directly into the shell command string.

Locations:

- `action.yml:80`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two unpinned-uses findings: both occurrences of `jdx/mise-action@v2` replaced with full SHA `jdx/mise-action@c37c93293d6b742fc901e1406b8f764f6fb19dac # v2`. Fixed script-injection finding: moved `${{ github.action_path }}` from the `run:` command string into an `env:` block as `ACTION_PATH`, and updated the shell command to reference `$ACTION_PATH/src/main.sh`.

