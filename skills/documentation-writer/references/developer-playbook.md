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

**Step 3 — Generate content.** Use the templates in [templates/developer/](../templates/developer/). For the three mandatory output files (`README.md`, `user-playbook.md`, `developer-playbook.md`), generate directly without awaiting approval — the output format is fixed. For supplementary documents (ADRs, add-a-feature guides, changelogs, agent-context files) propose the table of contents first and await approval before writing full content. For single-unit output (one docstring, one ADR) generate directly. For ASCII flowchart syntax, see [ascii-diagram-guide.md](ascii-diagram-guide.md).

---

## Depth Requirements

### Core functions reference standard
When documenting a module's functions, the reference is only complete when:
- Every public function (not prefixed `_`) is listed — no omissions
- Each entry has: function name, parameter table, return type, raised exceptions, and a usage example (the only allowed code fence per function)
- Pipeline stages, orchestrators, and functions with ordering constraints include a **Flow** ASCII diagram (input → process → output) before the parameter table
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
- **Diagram-first** — an end-to-end ASCII workflow overview (create → configure → wire → test → deploy) before step prose
- **End-to-end** — covers every file that must be created or modified, in order
- **Concrete** — includes the exact directory to create files in, the exact sections of config files to edit, and the exact commands to run to verify the new feature works
- **Non-redundant** — does not say "add your logic here"; points to an existing example in the codebase
- **Checkpointed** — each step has a verification command or observable output so the developer knows they are on track before proceeding
- **Brief prose** — max ~3 sentences under the workflow diagram; step details in tables and short labels

---

## Templates

Use these as starting shapes. Read the linked file before generating content.

| Template | File | Notes |
|---|---|---|
| README index | [readme-index.md](../templates/developer/readme-index.md) | Mandatory `README.md` shape |
| Core Functions Reference | [core-functions-reference.md](../templates/developer/core-functions-reference.md) | Per-module API catalog |
| Add-a-Feature Guide | [add-a-feature.md](../templates/developer/add-a-feature.md) | End-to-end extension workflow |
| ADR | [adr.md](../templates/developer/adr.md) | Store as `docs/decisions/ADR-NNN-slug.md`. Number sequentially. Never delete — permanent record. Lifecycle: PROPOSED → ACCEPTED → (SUPERSEDED or DEPRECATED). Cross-link when superseding. |
| API Doc (Python) | [api-doc-python.md](../templates/developer/api-doc-python.md) | Docstring shape |
| API Doc (TypeScript) | [api-doc-typescript.md](../templates/developer/api-doc-typescript.md) | JSDoc shape |
| API Doc (OpenAPI) | [api-doc-openapi.md](../templates/developer/api-doc-openapi.md) | REST endpoint shape |
| Inline Gotcha | [inline-gotcha.md](../templates/developer/inline-gotcha.md) | Comment the *why*, not the *what* |
| Changelog Entry | [changelog-entry.md](../templates/developer/changelog-entry.md) | [Keep a Changelog](https://keepachangelog.com/) format |
| Agent-Context File | [agent-context.md](../templates/developer/agent-context.md) | `CLAUDE.md`, `AGENTS.md`, `.cursor/rules/` |
| Not applicable stub | [not-applicable.md](../templates/not-applicable.md) | When track has no developer content |

**Inline gotcha — bad vs good:**

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
- Sequential logic described in long prose without an ASCII flowchart (3+ steps or branching)
- Prose under a diagram that restates nodes or edges already shown
- Diagram nodes or edges not verifiable from source material
- Mermaid blocks in output (use ASCII in `text` fences instead)

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
- [ ] Module-level and pipeline-stage ASCII flowcharts present where required
- [ ] Every 3+ step or branching process opens with an ASCII flowchart before prose
- [ ] Prose under each diagram is ≤ ~3 sentences and does not restate diagram content
- [ ] Add-a-feature guide opens with workflow overview ASCII diagram
- [ ] No Mermaid blocks in output

---

## Additional resources

- ASCII diagram syntax: [ascii-diagram-guide.md](ascii-diagram-guide.md)
- Skill contracts and execution order: [SKILL.md](../SKILL.md)
