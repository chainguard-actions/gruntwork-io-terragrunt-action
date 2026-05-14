# Hardening Report: gruntwork-io--terragrunt-action/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses `jdx/mise-action@v2` in two steps, both pinned to a mutable version tag (`@v2`) rather than an immutable 40-character commit SHA. This exposes the action to supply-chain attacks if the tag is moved to point to malicious code.

Locations:

- `action.yml:67`
- `action.yml:74`

### script-injection (severity: high)

The 'Execute Terragrunt' step directly interpolates the `github.action_path` context expression inside a `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. GitHub Actions expressions from the `github.*` context should be assigned to an environment variable first and then referenced as `$ENV_VAR` in the shell command, rather than being interpolated directly into the command string.

Locations:

- `action.yml:84`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two security findings in action.yml: (1) Pinned both `jdx/mise-action@v2` references to the immutable commit SHA `c37c93293d6b742fc901e1406b8f764f6fb19dac` with `# v2` comment for readability. (2) Moved `${{ github.action_path }}` out of the `run:` shell command and into the step's `env:` block as `ACTION_PATH`, then referenced it as `$ACTION_PATH/src/main.sh` in the shell script to prevent script injection.

