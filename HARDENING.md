<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is interpolated directly inside a `run:` shell command string. The step 'Execute Terragrunt' uses `run: ${{ github.action_path }}/src/main.sh`, which injects the github.action_path context value directly into the shell command before the shell ever sees it. Per the check rules, ANY ${{ ... }} expression inside a run: block is a script-injection finding regardless of which context it reads from. The fix is to use the $GITHUB_ACTION_PATH environment variable instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`

Locations:

- `action.yml:88`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script-injection in action.yml line 88: replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"`. The $GITHUB_ACTION_PATH environment variable is automatically set by GitHub Actions to the same value as github.action_path, but using it avoids direct ${{ }} expression interpolation inside the run: shell command string.

