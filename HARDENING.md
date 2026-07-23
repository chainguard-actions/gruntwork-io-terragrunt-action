<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.4.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: A GitHub Actions expression `${{ github.action_path }}` is interpolated directly inside a `run:` shell command string. The offending line is `run: ${{ github.action_path }}/src/main.sh`. Even though `github.action_path` is not attacker-controlled in the same way as `github.head_ref`, any `${{ ... }}` expression directly inside a `run:` block undergoes YAML template substitution before the shell sees it, making it a script-injection risk. The value should be passed via an environment variable and referenced as `$GITHUB_ACTION_PATH` instead.

Locations:

- `action.yml:102`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed the script-injection finding in action.yml at line 102. Replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"`. GitHub Actions automatically sets the `GITHUB_ACTION_PATH` environment variable to the action's path, so this is a safe and equivalent replacement that avoids YAML template substitution of a `${{ }}` expression directly inside a `run:` shell command string.

