# Developer Playbook

Developer documentation lives inside the codebase and serves engineers and AI agents working on it.

---

## Document Types

| Type | Purpose | Default location |
|---|---|---|
| ADR | Capture architectural decisions and their rationale | `docs/decisions/ADR-NNN-slug.md` |
| API / function doc | Document public function and class contracts | Inline (docstring / JSDoc) or OpenAPI |
| Core functions reference | Comprehensive catalog of all public functions per module | `docs/api/<module>.md` or inline |
| README | Index page: one-paragraph description + links to `user-playbook.md` and `developer-playbook.md`. Max 60 lines. | `README.md` at repo root |
| Add-a-feature guide | Step-by-step instructions for extending the codebase | `docs/contributing/add-<feature-type>.md` |
| Inline gotcha | Warn engineers/agents of known traps | Comment at the trap site |
| Changelog entry | Record user-visible changes per release | `CHANGELOG.md` |
| Agent-context file | Give AI agents project conventions | `CLAUDE.md`, `AGENTS.md`, `.cursor/rules/` |

---

## Workflow

**Step 1 — Identify the document type.** Use the table above. If the request is ambiguous, infer from context:
- A function, method, or class → Core functions reference / API doc
- A design choice between competing options → ADR
- A project someone needs to run → README
- "How do I add X" → Add-a-feature guide
- A non-obvious behavior or constraint → Inline gotcha
- A shipped release → Changelog entry
- Project-wide conventions for an agent → Agent-context file

**Step 2 — Derive from source material.** Read every provided file. Extract function signatures, parameter names, types, return values, config keys, and file paths directly. Present API details in parameter tables and prose — do not emit fenced signature blocks in the three mandatory output files.

Ask one targeted question per missing piece if something critical is absent. Do not write any section that depends on a gap.

**Step 3 — Generate content.** Use the templates below. For the three mandatory output files (`README.md`, `user-playbook.md`, `developer-playbook.md`), generate directly without awaiting approval — the output format is fixed. For supplementary documents (ADRs, add-a-feature guides, changelogs, agent-context files) propose the table of contents first and await approval before writing full content. For single-unit output (one docstring, one ADR) generate directly.

---

## Depth Requirements

### Core functions reference standard
When documenting a module's functions, the reference is only complete when:
- Every public function (not prefixed `_`) is listed — no omissions
- Each entry has: function name, parameter table, return type, raised exceptions, and a usage example (the only allowed code fence per function)
- Pipeline stages, orchestrators, and functions with ordering constraints include a **Flow** Mermaid diagram (input → process → output) before the parameter table
- Side effects (writes to disk, mutates state, makes network calls) are explicitly documented
- Any ordering constraint ("must be called before X") is shown as diagram edges, not long prose
- If the function is a pipeline stage entry point, document what it consumes and what it produces (input table → output table, or input file → output file)

### README index standard
`README.md` is an index file, not a deep onboarding doc. It is complete when:

**Required (must be present):**
- Project title as an h1 heading — exact project name from source material
- One paragraph describing what the project does and who it is for — derived from source, not invented
- A link to `user-playbook.md` with a one-line description: "How to use this project"
- A link to `developer-playbook.md` with a one-line description: "How to work on this project"

**Line budget:** The entire file must be ≤60 lines. Stop writing when the four required elements are present.

**Prohibited in README only (do not add to README.md):**
- Quick-start commands or installation instructions
- Environment variable listings
- Architecture diagrams or module tables
- Contributing or PR process sections
- Changelog links or release notes
- Any section that belongs in `developer-playbook.md` or `user-playbook.md`

Architecture and workflow flowcharts belong in `developer-playbook.md`, not `README.md`.

If you are tempted to add any prohibited content, put it in the appropriate playbook instead.

### Add-a-feature guide standard
This is the highest-value document for a growing codebase. It must be:
- **Diagram-first** — an end-to-end Mermaid workflow overview (create → configure → wire → test → deploy) before step prose
- **End-to-end** — covers every file that must be created or modified, in order
- **Concrete** — includes the exact directory to create files in, the exact sections of config files to edit, and the exact commands to run to verify the new feature works
- **Non-redundant** — does not say "add your logic here"; points to an existing example in the codebase
- **Checkpointed** — each step has a verification command or observable output so the developer knows they are on track before proceeding
- **Brief prose** — max ~3 sentences under the workflow diagram; step details in tables and short labels

---

## Templates

### ADR

Store as `docs/decisions/ADR-NNN-slug.md`. Number sequentially. Never delete — they are permanent record.

```markdown
# ADR-NNN: [Short decision title]

## Status
Proposed | Accepted | Superseded by ADR-NNN | Deprecated

## Date
YYYY-MM-DD

## Context
The situation, constraints, and requirements that forced a decision.
Be specific — include scale, team size, operational constraints.

## Decision
One clear sentence: what was decided.

## Alternatives Considered

### [Option A]
- Pros: ...
- Cons: ...
- Rejected because: ...

### [Option B]
- Pros: ...
- Cons: ...
- Rejected because: ...

## Consequences
What becomes easier. What becomes harder. What new obligations this creates.
```

**Lifecycle:** PROPOSED → ACCEPTED → (SUPERSEDED or DEPRECATED). When a decision changes, write a new ADR and cross-link both directions.

---

### Core Functions Reference (per module)

Produce one section per public function. Use the function name and parameter table — do not fence the full signature.

```markdown
# [Module Name] — Function Reference

## Module flow

\`\`\`mermaid
flowchart LR
  input[Input] --> stageOne[stage_one]
  stageOne --> stageTwo[stage_two]
  stageTwo --> output[Output]
\`\`\`

[Max 3 sentences. Required when module has pipeline stages or 3+ linked functions.]

---

## function_name

`function_name(param1: Type, param2: Type = default) -> ReturnType`

[One-sentence description of what this function does.]

### Flow
[Include when this function is a pipeline stage or orchestrator with 3+ steps or branching.]

\`\`\`mermaid
flowchart TD
  consume[Consume input] --> transform[Transform]
  transform --> produce[Produce output]
\`\`\`

[Max 3 sentences. Do not restate diagram nodes.]

### Parameters
| Name | Type | Required | Default | Description |
|---|---|---|---|---|
| param1 | Type | Yes | — | [What it is and valid values] |
| param2 | Type | No | default | [Effect of changing it] |

### Returns
`ReturnType` — [Description of what is returned and its shape.]

### Raises
| Exception | When |
|---|---|
| `ExceptionType` | [Condition that triggers it] |

### Side effects
[Any writes to disk, mutations, or network calls. Write "None" if there are none.]

### Usage example
\`\`\`
result = function_name(param1=value, param2=other_value)
\`\`\`

---
[Repeat for next function]
```

---

### README

This is an index file. Follow the README index standard above. The complete file must stay within 60 lines.

```markdown
# [Project Name]

[One paragraph: what this project does and who it is for. Derive from provided source material — do not invent.]

## Documentation

- [User Playbook](user-playbook.md) — How to use this project: tutorials, how-to guides, reference, and explanations for end users.
- [Developer Playbook](developer-playbook.md) — How to work on this project: API reference, architecture decisions, add-a-feature guides, and engineering conventions.
```

---

### Add-a-Feature Guide

```markdown
# How to Add [Feature Type]

## Overview
What this guide produces. Which existing feature this mirrors (reference implementation).

## Workflow overview

\`\`\`mermaid
flowchart TD
  createModule[Create module] --> registerConfig[Register config]
  registerConfig --> wirePipeline[Wire into pipeline]
  wirePipeline --> writeTests[Write tests]
  writeTests --> deployValidate[Deploy and validate]
\`\`\`

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
\`\`\`bash
[Exact deploy command]
[Exact validation command or check]
\`\`\`

## Checklist
- [ ] Module created in correct directory
- [ ] Configuration entry added
- [ ] Pipeline/job definition updated
- [ ] Tests written and passing
- [ ] Deployed and validated in dev
```

---

### API Doc (Python docstring)

```python
def function_name(param1: Type, param2: Type = default) -> ReturnType:
    """One-line description of what this function does.

    Args:
        param1: Description including valid range or shape.
        param2: Description. Defaults to `default`.

    Returns:
        Description of what is returned and its shape.

    Raises:
        ExceptionType: Condition that triggers this exception.

    Example:
        >>> result = function_name(param1=value)
        >>> print(result)
        expected_output
    """
```

---

### API Doc (TypeScript / JSDoc)

```typescript
/**
 * One-line description of what this function does.
 *
 * @param paramName - Description including valid range or shape
 * @returns Description of what is returned
 * @throws {ErrorType} When and why this is thrown
 *
 * @example
 * const result = await myFunction({ id: 'abc123' });
 * console.log(result.status); // "ok"
 */
export async function myFunction(input: InputType): Promise<OutputType> { ... }
```

---

### API Doc (OpenAPI / REST)

```yaml
/api/resource:
  post:
    summary: [One-line description]
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/CreateInput'
    responses:
      '201':
        description: Resource created
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Resource'
      '422':
        description: Validation error
```

---

### Inline Gotcha

Comment the *why*. Never comment the *what* — code that restates itself is noise.

```
# IMPORTANT: [One sentence stating the trap.]
# [One to two sentences explaining why it exists and what breaks if ignored.]
# See ADR-NNN or [file] for full rationale.
```

**Bad vs good:**

```python
# BAD — restates the code
counter += 1  # increment counter by 1

# GOOD — explains non-obvious constraint
# Sliding window resets at the boundary, not on a fixed schedule,
# to prevent burst attacks at window edges (see ADR-007).
if now - window_start > WINDOW_SIZE_MS:
    counter = 0
    window_start = now
```

---

### Changelog Entry

Follow [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
## [X.Y.Z] - YYYY-MM-DD
### Added
- Feature name: brief description (#issue-number)

### Fixed
- Bug description (#issue-number)

### Changed
- What changed and why (#issue-number)

### Removed
- What was removed (#issue-number)
```

---

### Agent-Context File (CLAUDE.md / AGENTS.md / .cursor/rules/)

```markdown
# [Project Name] — Agent Context

## What this project does
[One paragraph. What problem it solves, who uses it.]

## Stack
- Language/runtime: ...
- Key frameworks: ...
- Database / storage: ...
- Infrastructure: ...

## Coding conventions
- [Convention not enforced by linter]
- [Naming pattern]
- [File organization rule]

## Preferred libraries
- [Task] → use [library], not [alternative]

## What NOT to do
- Do not [anti-pattern 1 — include traps that would cause subtle failures]
- Do not [anti-pattern 2]
- [Trap or constraint]: [what goes wrong and why — fold gotcha guidance here, not in a separate section]

## Architecture decisions
Key decisions are recorded in `docs/decisions/`. Check there before re-litigating settled choices.
```

---

## Red Flags (flag and offer to fix)

- Any public function undocumented or partially documented
- README that cannot be followed to a running system from scratch
- Add-a-feature guide that says "add your logic here" without a concrete example
- Config keys documented that do not exist in the actual config files
- Architectural decisions with no written rationale
- Commented-out code (delete — git has history)
- TODO comments older than one sprint
- Comments that restate what the code says rather than explaining why
- Agent-context file that is outdated or missing
- Repo provenance citations in output (`(from ...)`, `> Source: ...`)
- Fenced implementation code (signatures, skeletons, copied config) in `user-playbook.md` or `developer-playbook.md`
- `## Known issues`, `## Known conflicts`, `## Known issues and conflicts`, `## Known gotchas`, or equivalent standalone conflict/issue sections
- Sequential logic described in long prose without a Mermaid flowchart (3+ steps or branching)
- Prose under a diagram that restates nodes or edges already shown
- Diagram nodes or edges not verifiable from source material

---

## Verification Checklist

Run this checklist for developer-track content, then run the **skill-level VERIFICATION checklist** in `SKILL.md` before delivering any output.

- [ ] Document type identified before generating
- [ ] API names, types, and defaults match source — presented in parameter tables/prose, not fenced signature blocks
- [ ] All config keys and paths exist in the provided files — none invented
- [ ] Core functions reference covers every public function with parameter table and usage example
- [ ] README can be followed from clone to running system without external help
- [ ] Add-a-feature guide has a verification step for every stage
- [ ] ADRs cross-linked when superseding old decisions
- [ ] Gotchas annotated inline at the trap site
- [ ] No commented-out code remains
- [ ] Agent-context files include "what NOT to do" section
- [ ] No repo provenance citations in `user-playbook.md` or `developer-playbook.md`
- [ ] No prohibited code fences (only usage syntax and shell commands)
- [ ] No standalone known-issues/conflicts/gotchas sections in mandatory output files
- [ ] Module-level and pipeline-stage Mermaid flowcharts present where required
- [ ] Every 3+ step or branching process opens with a Mermaid flowchart before prose
- [ ] Prose under each diagram is ≤ ~3 sentences and does not restate diagram content
- [ ] Add-a-feature guide opens with workflow overview Mermaid diagram
