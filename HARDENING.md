<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ ... }} expression is directly interpolated inside a `run:` shell command string. In the 'Execute Terragrunt' step, the line `run: ${{ github.action_path }}/src/main.sh` embeds `${{ github.action_path }}` directly into the shell command. Per the script-injection check, any `${{ ... }}` expression inside a `run:` block is a finding — the value flows through YAML template substitution before the shell ever sees it. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`

Locations:

- `action.yml:88`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

In the 'Execute Terragrunt' step of action.yml (line 88), replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"`. The built-in `$GITHUB_ACTION_PATH` environment variable is automatically set by GitHub Actions to the same value as `github.action_path`, so behavior is identical but without the script-injection risk of YAML template substitution.

