<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The 'Execute Terragrunt' step uses a ${{ }} expression directly inside a `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. Any GitHub Actions expression interpolated directly into a run: block is a script-injection risk because the value is substituted by the Actions template engine before the shell ever sees it. Even though `github.action_path` is not attacker-controlled in the same way as `github.head_ref`, the check rules require that NO `${{ ... }}` expression appear anywhere inside a run: shell command string. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:95`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in action.yml line 95: replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"`. The built-in `$GITHUB_ACTION_PATH` environment variable is equivalent to `${{ github.action_path }}` but is resolved by the shell rather than the Actions template engine, eliminating the injection risk.

