<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two composite action steps reference `jdx/mise-action@v2` using a mutable tag (`@v2`) instead of a pinned 40-character SHA commit hash. This exposes the action to supply-chain attacks if the tag is moved to point to a different (potentially malicious) commit. Both occurrences should be replaced with a full SHA pin, e.g. `jdx/mise-action@<40-char-sha> # v2`.

Locations:

- `action.yml:63`
- `action.yml:70`

### script-injection (severity: high)

Sub-rule (a): The 'Execute Terragrunt' step uses a `${{ ... }}` expression directly inside a `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. Any `${{ ... }}` expression interpolated directly into a `run:` block is a script-injection risk because the value is substituted into the shell command string before the shell parses it. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:79`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed three issues in hardened/action/action.yml: (1) Pinned both occurrences of `jdx/mise-action@v2` to the full SHA `c37c93293d6b742fc901e1406b8f764f6fb19dac` with `# v2` comment for readability. (2) Replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"` to eliminate the script-injection risk by using the pre-set `$GITHUB_ACTION_PATH` environment variable instead of a `${{ }}` expression interpolated directly into the shell command string.

