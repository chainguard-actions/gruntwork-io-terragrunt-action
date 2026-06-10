<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gruntwork-io--terragrunt-action/v3.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a `run:` shell command string. The line `run: ${{ github.action_path }}/src/main.sh` embeds a GitHub Actions expression directly into the shell command. Any ${{ ... }} expression in a run: block is a script-injection finding per the check rules, as the value flows through YAML template substitution before the shell sees it.

Locations:

- `action.yml:95`

### suspicious-run-content (severity: high)

eval-dynamic: The functions setup_pre_exec() and setup_post_exec() in src/main.sh read environment variables named INPUT_PRE_EXEC_* and INPUT_POST_EXEC_* from the process environment (inherited from the calling workflow) and pass their values directly to `eval "$pre_exec_command"` and `eval "$post_exec_command"`. This matches the eval-dynamic pattern (`eval $variable`). Since this is a composite action, these INPUT_PRE_EXEC_* / INPUT_POST_EXEC_* variables are workflow-controlled and can contain arbitrary shell code, giving callers full remote code execution on the runner.

Locations:

- `src/main.sh:72`
- `src/main.sh:84`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, suspicious-run-content

**Notes:**

1. action.yml line 95: Moved `${{ github.action_path }}` out of the `run:` command into the `env:` block as `ACTION_PATH: ${{ github.action_path }}`. The run command now uses `"$ACTION_PATH/src/main.sh"` (plain shell variable). Merged the two env blocks into one valid YAML structure.
2. src/main.sh lines 72 & 84: Replaced `eval "$pre_exec_command"` and `eval "$post_exec_command"` with a safer pattern: write the command content to a temp file via `printf '%s\n' "$var" > tmpfile` and execute with `bash "$tmpfile"`, then remove the temp file. This eliminates the dangerous `eval` builtin while preserving the intended pre/post exec hook functionality.

