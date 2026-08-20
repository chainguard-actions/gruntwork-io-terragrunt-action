<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in action.yml pin to the mutable tag `@v2` instead of a full 40-character commit SHA. This means the action could silently pull in a different (potentially malicious) version of jdx/mise-action on any future run. Both occurrences: (1) 'Install tools with mise (using mise.toml)' step and (2) 'Install tools with mise (using input versions)' step both use `uses: jdx/mise-action@v2`. These should be pinned to a specific SHA, e.g. `uses: jdx/mise-action@<40-char-sha> # v2`.

Locations:

- `action.yml:62`
- `action.yml:70`

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string. The step 'Execute Terragrunt' contains `run: ${{ github.action_path }}/src/main.sh`. Even though `github.action_path` is GitHub-controlled, any `${{ ... }}` expression directly inside a `run:` block undergoes YAML template substitution before the shell ever sees it, making it a script-injection risk. The value should instead be accessed via an environment variable (e.g. `env: ACTION_PATH: ${{ github.action_path }}` and then `run: "$ACTION_PATH"/src/main.sh`).

Locations:

- `action.yml:82`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed three issues in hardened/action/action.yml: (1) Pinned both occurrences of `jdx/mise-action@v2` to the full SHA `c37c93293d6b742fc901e1406b8f764f6fb19dac` with `# v2` comment for readability. (2) Moved `${{ github.action_path }}` out of the `run:` shell string in the 'Execute Terragrunt' step into an `env:` variable `ACTION_PATH`, then referenced it as `$ACTION_PATH/src/main.sh` in the shell command to eliminate the script-injection risk.

