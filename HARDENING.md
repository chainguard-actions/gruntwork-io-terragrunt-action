<!-- markdownlint-disable -->

# Hardening Report: gruntwork-io--terragrunt-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gruntwork-io--terragrunt-action/v3.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): a GitHub Actions expression is placed directly in a run: command in the 'Execute Terragrunt' step: `run: ${{ github.action_path }}/src/main.sh`. The template is filled in before the shell parses the command, and the path is neither quoted nor routed through env. Use the GITHUB_ACTION_PATH environment variable inside quotes instead: `run: "$GITHUB_ACTION_PATH/src/main.sh"`.

Locations:

- `action.yml:102`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 3d0e326..aa2bc1c 100644
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
         version: 2026.3.9
       env:
         MISE_GITHUB_TOKEN: ${{ inputs.github_token || github.token }}
-
     - name: Install tools with mise (using input versions)
       if: steps.mise-check.outputs.mise_config_exists == 'false'
       uses: jdx/mise-action@c1ecc8f748cd28cdeabf76dab3cccde4ce692fe4 # v4
       with:
         version: 2026.3.9
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

In action.yml, the 'Execute Terragrunt' step now runs `run: "\"$GITHUB_ACTION_PATH/src/main.sh\""`. After YAML parsing, bash gets the path in double quotes, which is stricter than the previous harden's YAML-quoted version (that one reached the shell unquoted). I also quoted $GITHUB_OUTPUT in the mise-check step. The previous patch showed a hunk adding lines under 'steps:' without listing their content, so those lines couldn't be re-created. Its other changes were blank-line and whitespace cleanup only, so I left them out. Nothing was run or tested after the edits.

