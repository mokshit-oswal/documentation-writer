# How to Add [Feature Type]

## Overview
What this guide produces. Which existing feature this mirrors (reference implementation).

## Workflow overview

```text
[Create module] --> [Register config] --> [Wire into pipeline]
       --> [Write tests] --> [Deploy and validate]
```

[Max 3 sentences caption. Do not restate diagram nodes.]

## Prerequisites
- [Tool or access required]
- [Pre-condition: e.g., "the project must already be running locally"]

## Step 1: Create the module
Location: `src/[exact/directory/]`
File name convention: `[naming pattern from existing modules]`

Create `src/[path]/[name].py` with the required functions and classes described below:
- `[function_or_class_name]`: [what it must do, derived from the reference implementation]
- `[function_or_class_name]`: [same pattern]

Verification: `[command to run]` → Expected: `[observable output]`

## Step 2: Register the configuration
File: `[exact config file path]`
Section: `[exact section name]`

Add an entry with these keys (match the shape of an existing entry in the same section):
| Key | Value / type | Description |
|---|---|---|
| [key-name] | [type or example value] | [what it controls] |

Verification: `[command]` → Expected: `[output]`

## Step 3: Wire it into the pipeline / job definition
File: `[exact file path]`

[Describe the exact change — which field to add or update, and what value to set. Point to the equivalent existing entry as the pattern to follow.]

Verification: `[command]` → Expected: `[output]`

## Step 4: Write tests
Location: `tests/[path]/`
[Describe what to test — what inputs, what assertions — based on the test pattern used by existing modules]

Verification: `[test command]` → Expected: all tests pass

## Step 5: Deploy and validate
```bash
[Exact deploy command]
[Exact validation command or check]
```

## Checklist
- [ ] Module created in correct directory
- [ ] Configuration entry added
- [ ] Pipeline/job definition updated
- [ ] Tests written and passing
- [ ] Deployed and validated in dev
