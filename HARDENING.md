<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command string. In the 'Execute Terragrunt' step, the run: value is `run: ${{ github.action_path }}/src/main.sh`. Any ${{ ... }} expression directly inside a run: block is a script-injection risk because the value is substituted by the YAML template engine before the shell ever sees it, bypassing shell quoting. The safe alternative is to use the $GITHUB_ACTION_PATH environment variable instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:88`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in action.yml at line 88 in the 'Execute Terragrunt' step. Replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"`. The $GITHUB_ACTION_PATH environment variable is the safe built-in alternative that is resolved by the shell rather than by the GitHub Actions template engine, eliminating the injection risk.

