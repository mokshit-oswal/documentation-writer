# User Playbook

User-facing documentation follows the [Diátaxis framework](https://diataxis.fr/). Every document falls into exactly one of the four quadrants.

---

## The Four Quadrants

| Type | Orientation | Analogy | When to use |
|---|---|---|---|
| **Tutorial** | Learning | A lesson | Newcomer needs a guided, successful first experience |
| **How-to Guide** | Problem-solving | A recipe | User knows what they want and needs steps to do it |
| **Reference** | Information | A dictionary | User needs accurate, complete technical facts |
| **Explanation** | Understanding | A discussion | User needs to understand *why* something works the way it does |

---

## Workflow

**Step 1 — Clarify (minimum questions only)**

Only ask for what is genuinely unknown. Skip any question the user already answered.

| Question | Ask if... |
|---|---|
| Which quadrant? | The user did not specify (e.g. "tutorial", "how-to", "reference", "guide") |
| Target audience? | Not clear from context |
| User's goal? | Not inferable from the request |
| Scope — what's in/out? | The topic is broad enough that omissions could mislead |

Never ask more than two clarifying questions at once.

**Step 2 — Generate content**

For `user-playbook.md` (the mandatory output file), generate content directly — the output format is fixed, no approval gate. For other user-facing supplementary documents requested beyond the three mandatory files, draft a table of contents with one-line descriptions per section and await approval before writing full content.

**Step 3 — Write the content**

Write the content in well-formatted Markdown using the templates in [templates/user/](../templates/user/) as starting shapes. All details must be derived from provided code, config files, or existing docs — never invented. For ASCII flowchart syntax, see [ascii-diagram-guide.md](ascii-diagram-guide.md).

---

## Depth Requirements

User documentation must be clear enough that a reader can understand and run the project without asking anyone. Apply these standards to every document:

### Core features & component overview (primary content for user-playbook.md)
When documenting a project, always lead with:
1. **What the project does** — one short paragraph, plain language, no file paths
2. **System overview flowchart** — an ASCII diagram in a `text` fence showing how user-facing components interact and how data or tasks move between them (required when the project has 2+ components or a multi-step user flow)
3. **Core features** — a bulleted list of the key capabilities the user gets
4. **Component descriptions** — a dedicated subsection per component covering:
   - An ASCII flowchart when the component involves 3+ steps or branching — placed before bullets
   - Max ~3 sentences of prose under the diagram (do not restate diagram nodes)
   - How to invoke it (the command to run or the job to trigger)
   - What the user provides and what they get back
   - Do not reference internal file paths or module locations

### How-to depth standard
A how-to guide is only complete when:
- A **decision/path flowchart** (ASCII in a `text` fence) appears before numbered steps when the task has 3+ steps or any branching
- Numbered steps are brief labels matching diagram nodes — each step has a concrete command or action
- Every step states the expected observable result
- The "Verify it worked" section gives a specific check, not just "check that it succeeded"

### Explanation depth standard
When the concept involves flow, state, or cause-and-effect:
- **How it works** opens with an ASCII concept-model flowchart before prose
- Prose under the diagram is ≤ ~3 sentences; trade-offs stay brief

### Reference depth standard
A reference page is only complete when:
- Every parameter is listed — no "and others"
- Each parameter has: name, type, required/optional, default value (from source), and a description of effect
- At least one minimal usage example and one full usage example are provided (usage syntax only — not implementation code)
- All valid values for enum-like parameters are enumerated

---

## Templates

Use these as starting shapes. Read the linked file before generating content.

| Template | File |
|---|---|
| Core Features & Component Overview (lead section of every `user-playbook.md`) | [core-features-overview.md](../templates/user/core-features-overview.md) |
| Tutorial | [tutorial.md](../templates/user/tutorial.md) |
| How-to Guide | [how-to.md](../templates/user/how-to.md) |
| Reference | [reference.md](../templates/user/reference.md) |
| Explanation | [explanation.md](../templates/user/explanation.md) |
| Not applicable stub (when track has no user content) | [not-applicable.md](../templates/not-applicable.md) |

---

## Red Flags (flag and offer to fix)

- Core features or components missing from the overview
- File paths or module names appearing in user-facing sections
- `## Common misconceptions` or `## Troubleshooting` sections present in the output
- `## Known issues`, `## Known conflicts`, `## Known issues and conflicts`, `## Known gotchas`, or equivalent standalone conflict/issue sections
- Repo provenance citations in output (`(from ...)`, `> Source: ...`)
- Fenced implementation code (signatures, skeletons, copied config) — only usage syntax and shell commands are allowed
- Config keys documented that do not exist in the actual config files
- Steps that say "configure X" without specifying exactly how
- Vague "verify it worked" sections (e.g., "check that it succeeded")
- Reference tables with missing defaults or missing parameter entries
- Documentation that mixes quadrants (e.g., a tutorial that also tries to be a reference)
- Tutorials that don't lead to a concrete, observable outcome
- Sequential logic described in long prose without an ASCII flowchart (3+ steps or branching)
- Prose under a diagram that restates nodes or edges already shown
- Diagram nodes or edges not verifiable from source material
- Mermaid blocks in output (use ASCII in `text` fences instead)

---

## Verification Checklist

Run this checklist for user-track content, then run the **skill-level VERIFICATION checklist** in `SKILL.md` before delivering any output.

- [ ] `user-playbook.md` leads with a Core Features & Component Overview section
- [ ] System overview ASCII flowchart present when project has 2+ components or multi-step user flow
- [ ] Every 3+ step or branching process opens with an ASCII flowchart before prose
- [ ] Prose under each diagram is ≤ ~3 sentences and does not restate diagram content
- [ ] Project description uses plain language — no file paths, no module names
- [ ] Every core feature visible to the user is listed
- [ ] Every component has a dedicated subsection with: what it does, how to run it, inputs and outputs
- [ ] No `## Common misconceptions` section anywhere in the file
- [ ] No `## Troubleshooting` table anywhere in the file
- [ ] No standalone known-issues/conflicts/gotchas sections
- [ ] No repo provenance citations (`(from ...)`, `> Source: ...`)
- [ ] Code fences limited to usage syntax and shell commands — no implementation code
- [ ] Target audience and user goal are explicit
- [ ] For supplementary docs beyond the three mandatory files: structure was proposed and approved before full content was written
- [ ] All config keys and parameters are sourced from actual files — none invented
- [ ] Every step in how-to guides has a concrete action and an expected result
- [ ] Reference tables list every parameter with type, required flag, and actual default
- [ ] Reference Minimal/Full examples are usage syntax only — not implementation code
- [ ] Nothing invented — all content derived from provided source material
- [ ] No Mermaid blocks in output

---

## Additional resources

- ASCII diagram syntax: [ascii-diagram-guide.md](ascii-diagram-guide.md)
- Skill contracts and execution order: [SKILL.md](../SKILL.md)
