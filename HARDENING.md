<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.4.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a), direct expression interpolation: the 'Execute Terragrunt' step puts a GitHub Actions expression straight into its run: command: `run: ${{ github.action_path }}/src/main.sh`. The value is pasted into the script before the shell parses it, and it is unquoted. Fix: use the runner-provided env var with quotes instead, e.g. `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:104`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index f74bd21..71e7daa 100644
--- a/action.yml
+++ b/action.yml
@@ -1,11 +1,9 @@
 name: 'Gruntwork Terragrunt'
 description: 'Setup and execute Terragrunt.'
-author:  'Gruntwork'
-
+author: 'Gruntwork'
 branding:
   icon: 'award'
   color: 'purple'
-
 inputs:
   tg_version:
     description: 'Terragrunt version to install (required if no mise.toml file present).'
@@ -40,7 +38,6 @@ inputs:
   github_token:
     description: 'GitHub token for API authentication to avoid rate limits when installing tools'
     required: false
-
 outputs:
   tg_action_output:
     description: 'Terragrunt execution output'
@@ -48,10 +45,26 @@ outputs:
   tg_action_exit_code:
     description: 'Terragrunt exit code'
     value: ${{ steps.terragrunt-exec.outputs.tg_action_exit_code }}
-
 runs:
   using: 'composite'
   steps:
     - name: Check for mise.toml and validate inputs
       id: mise-check
       shell: bash
@@ -76,18 +89,16 @@ runs:
             exit 1
           fi
         fi
-
     - name: Install tools with mise (using mise.toml)
       if: steps.mise-check.outputs.mise_config_exists == 'true'
       uses: jdx/mise-action@c1ecc8f748cd28cdeabf76dab3cccde4ce692fe4 # v4
       with:
         version: 2026.7.7
       env:
         MISE_GITHUB_TOKEN: ${{ inputs.github_token || github.token }}
-
     - name: Install tools with mise (using input versions)
       if: steps.mise-check.outputs.mise_config_exists == 'false'
       uses: jdx/mise-action@c1ecc8f748cd28cdeabf76dab3cccde4ce692fe4 # v4
       with:
         version: 2026.7.7
         tool_versions: |
@@ -95,11 +106,10 @@ runs:
           ${{ inputs.tofu_version && format('opentofu {0}', inputs.tofu_version) || '' }}
       env:
         MISE_GITHUB_TOKEN: ${{ inputs.github_token || github.token }}
-
     - name: Execute Terragrunt
       if: ${{ inputs.tg_command != '' }}
       id: terragrunt-exec
-      run: ${{ github.action_path }}/src/main.sh
+      run: "$GITHUB_ACTION_PATH/src/main.sh"
       shell: bash
       env:
         TG_NON_INTERACTIVE: true

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, prior-fix

**Notes:**

In action.yml, the 'Execute Terragrunt' step no longer runs `${{ github.action_path }}/src/main.sh`. It now runs `"$GITHUB_ACTION_PATH/src/main.sh"`, written in the YAML as `run: "\"$GITHUB_ACTION_PATH/src/main.sh\""`. The extra escaping means the shell gets the path in double quotes. The previous harden wrote `run: "$GITHUB_ACTION_PATH/src/main.sh"`, where the YAML parser removes the quotes, so the shell got the path unquoted. This edit re-applies that fix with the quotes kept. Every other change in the previous patch only removed blank lines or extra spaces, so I didn't repeat those. One hunk in that patch was cut short and its added lines weren't shown, so I couldn't check what it changed. Nothing else in action.yml needed fixing: the script's inputs already come in through env:, and the mise actions are pinned by commit SHA.

