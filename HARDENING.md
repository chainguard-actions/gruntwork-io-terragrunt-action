<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references `jdx/mise-action@v2` (a mutable tag, not a pinned 40-character SHA commit hash) in two composite action steps. This is a supply-chain risk: if the tag is moved to a different commit, the action will silently execute different code. Both occurrences should be pinned to a full SHA digest, e.g. `jdx/mise-action@<40-char-sha> # v2`.

Locations:

- `action.yml:57`
- `action.yml:63`

### script-injection (severity: high)

Sub-rule (a): A `${{ }}` expression is interpolated directly inside a `run:` shell command string. The step `Execute Terragrunt` uses `run: ${{ github.action_path }}/src/main.sh`, which injects the `github.action_path` context value directly into the shell command before the shell ever sees it. Per the check rules, ANY `${{ ... }}` expression inside a `run:` block is a script-injection finding regardless of which context it reads from. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:72`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. Pinned both `jdx/mise-action@v2` references to full SHA `c37c93293d6b742fc901e1406b8f764f6fb19dac # v2` (lines 57 and 63). 2. Fixed script injection on line 72 by replacing `${{ github.action_path }}/src/main.sh` with `"$GITHUB_ACTION_PATH/src/main.sh"`, using the built-in `$GITHUB_ACTION_PATH` environment variable instead of a template expression interpolated directly into the shell command string.

