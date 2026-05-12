# Hardening Report: gruntwork-io--terragrunt-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml references `jdx/mise-action@v2` twice using a mutable tag (`@v2`) instead of a pinned 40-character SHA commit hash. This exposes the action to supply-chain attacks where the tag could be silently moved to point to malicious code.

Locations:

- `action.yml:57`
- `action.yml:63`

### script-injection (severity: high)

The 'Execute Terragrunt' step uses `run: ${{ github.action_path }}/src/main.sh`, directly interpolating the `github.action_path` expression inside a `run:` shell command string. Attacker-controlled GitHub context values should be assigned to an environment variable first and then referenced as `$ENV_VAR` in the shell command, rather than being interpolated directly via `${{ ... }}`.

Locations:

- `action.yml:75`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two security findings in action.yml: (1) Pinned both occurrences of `jdx/mise-action@v2` to the full commit SHA `c37c93293d6b742fc901e1406b8f764f6fb19dac` with `# v2` comment for readability. (2) Fixed script injection in the 'Execute Terragrunt' step by moving `${{ github.action_path }}` out of the `run:` shell string into an `ACTION_PATH` environment variable, and referencing it as `$ACTION_PATH/src/main.sh` in the shell command.

