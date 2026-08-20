<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both composite action steps that install tools reference `jdx/mise-action@v2`, which is a mutable tag rather than a pinned 40-character SHA commit hash. If the tag is moved (intentionally or via a supply-chain compromise), the action will silently execute different code. Both occurrences must be pinned to a full SHA, e.g. `jdx/mise-action@<40-char-sha> # v2`.

Locations:

- `action.yml:57`
- `action.yml:64`

### script-injection (severity: high)

Sub-rule (a): The 'Execute Terragrunt' step uses a `${{ }}` expression directly inside a `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. Any `${{ ... }}` interpolated directly into a `run:` block is a script-injection risk because the value is substituted into the shell command before the shell parses it. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:73`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed both occurrences of jdx/mise-action@v2 by pinning to full SHA c37c93293d6b742fc901e1406b8f764f6fb19dac (with # v2 comment). Fixed script-injection in the 'Execute Terragrunt' step by replacing `${{ github.action_path }}/src/main.sh` with `"$GITHUB_ACTION_PATH/src/main.sh"`, using the pre-set environment variable instead of a template expression directly in the shell command.

