<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references `jdx/mise-action@v2` twice using a mutable version tag instead of a full 40-character commit SHA. This means the action could be silently updated to a different (potentially malicious) version without any change to this repository.

Locations:

- `action.yml:62`
- `action.yml:70`

### script-injection (severity: high)

Rule (a) violation: The 'Execute Terragrunt' step uses `run: ${{ github.action_path }}/src/main.sh`, which directly interpolates a `${{ }}` expression inside a `run:` shell command string. Although `github.action_path` is not attacker-controlled in the same way as `github.head_ref`, any `${{ ... }}` expression directly inside a `run:` block is a script-injection finding per the check rules, as the value flows through YAML template substitution before the shell ever sees it. The safe pattern is to use the `$GITHUB_ACTION_PATH` environment variable instead.

Locations:

- `action.yml:82`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. Pinned both occurrences of `jdx/mise-action@v2` to full SHA `c37c93293d6b742fc901e1406b8f764f6fb19dac` with `# v2` comment for readability. 2. Replaced `run: ${{ github.action_path }}/src/main.sh` with `run: "$GITHUB_ACTION_PATH/src/main.sh"` to eliminate the script-injection finding by using the built-in environment variable instead of a template expression inside the shell command string.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Replaced `eval "$pre_exec_command"` and `eval "$post_exec_command"` in `src/main.sh` with `bash -c "$pre_exec_command"` and `bash -c "$post_exec_command"` respectively. The `eval` builtin performs double-expansion in the current shell context, allowing workflow-controlled input values to inject and execute arbitrary shell commands with access to the current shell's environment and state. Using `bash -c` instead spawns a child process and passes the command string as a distinct argument to bash's `-c` option, preventing the double-evaluation injection risk while preserving the intended functionality of executing the pre/post exec hook commands.

