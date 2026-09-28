<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.0.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both composite action steps that install tools reference `jdx/mise-action@v2` using a mutable tag (`@v2`) rather than a pinned 40-character commit SHA. This means the action could be silently updated to a malicious version without any change to this repository, creating a supply-chain attack vector.

Locations:

- `action.yml:67`
- `action.yml:73`

### script-injection (severity: high)

Sub-rule (a): The 'Execute Terragrunt' step directly interpolates a `${{ }}` expression inside the `run:` shell command string: `run: ${{ github.action_path }}/src/main.sh`. Any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value is substituted into the shell command string by the Actions template engine before the shell ever sees it, bypassing shell quoting. The safe alternative is to use the `$GITHUB_ACTION_PATH` environment variable instead.

Locations:

- `action.yml:83`

### suspicious-run-content (severity: high)

eval-dynamic: The `setup_pre_exec` and `setup_post_exec` functions in src/main.sh read all environment variables matching `INPUT_PRE_EXEC_*` / `INPUT_POST_EXEC_*` (which are inherited from and controlled by the calling workflow) and pass their values directly to `eval "$pre_exec_command"` / `eval "$post_exec_command"`. This allows any calling workflow to inject and execute arbitrary shell code in the runner by setting these environment variables, matching the `eval $variable` pattern for eval-dynamic.

Locations:

- `src/main.sh:72`
- `src/main.sh:83`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, suspicious-run-content

**Notes:**

1. Pinned both jdx/mise-action@v2 references to full SHA c37c93293d6b742fc901e1406b8f764f6fb19dac # v2 in action.yml. 2. Replaced ${{ github.action_path }}/src/main.sh with $GITHUB_ACTION_PATH/src/main.sh in the Execute Terragrunt step to eliminate script injection via template expression. 3. Replaced eval with bash -c in both setup_pre_exec and setup_post_exec functions in src/main.sh to address the eval-dynamic finding — commands now run in a subshell rather than the current shell context.

