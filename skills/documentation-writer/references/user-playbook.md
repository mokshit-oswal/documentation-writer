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

Write the content in well-formatted Markdown using the templates below as starting shapes. All details must be derived from provided code, config files, or existing docs — never invented.

---

## Depth Requirements

User documentation must be clear enough that a reader can understand and run the project without asking anyone. Apply these standards to every document:

### Core features & component overview (primary content for user-playbook.md)
When documenting a project, always lead with:
1. **What the project does** — one short paragraph, plain language, no file paths
2. **System overview flowchart** — a Mermaid diagram showing how user-facing components interact and how data or tasks move between them (required when the project has 2+ components or a multi-step user flow)
3. **Core features** — a bulleted list of the key capabilities the user gets
4. **Component descriptions** — a dedicated subsection per component covering:
   - A Mermaid flowchart when the component involves 3+ steps or branching — placed before bullets
   - Max ~3 sentences of prose under the diagram (do not restate diagram nodes)
   - How to invoke it (the command to run or the job to trigger)
   - What the user provides and what they get back
   - Do not reference internal file paths or module locations

### How-to depth standard
A how-to guide is only complete when:
- A **decision/path flowchart** (Mermaid) appears before numbered steps when the task has 3+ steps or any branching
- Numbered steps are brief labels matching diagram nodes — each step has a concrete command or action
- Every step states the expected observable result
- The "Verify it worked" section gives a specific check, not just "check that it succeeded"

### Explanation depth standard
When the concept involves flow, state, or cause-and-effect:
- **How it works** opens with a Mermaid concept-model flowchart before prose
- Prose under the diagram is ≤ ~3 sentences; trade-offs stay brief

### Reference depth standard
A reference page is only complete when:
- Every parameter is listed — no "and others"
- Each parameter has: name, type, required/optional, default value (from source), and a description of effect
- At least one minimal usage example and one full usage example are provided (usage syntax only — not implementation code)
- All valid values for enum-like parameters are enumerated

---

## Templates

### Core Features & Component Overview (use this as the lead section of every user-playbook.md)

```markdown
# [Project Name]

## What this project does
[One to two sentences describing the purpose of the project in plain language. No file paths or module names.]

## How it fits together

\`\`\`mermaid
flowchart LR
  userInput[User input] --> componentA[Component A]
  componentA --> componentB[Component B]
  componentB --> userOutput[User output]
\`\`\`

[Max 3 sentences: caption for the diagram. Do not restate nodes already shown.]

## Core features
- [Feature 1]: [One sentence describing what it does for the user]
- [Feature 2]: [One sentence]
- [Feature 3]: [One sentence]

## Components

### [Component name]

\`\`\`mermaid
flowchart TD
  start[Start] --> stepOne[Step one]
  stepOne --> stepTwo[Step two]
  stepTwo --> endNode[Result]
\`\`\`

[Max 3 sentences. How to invoke — command or job name. Inputs and outputs.]

### [Component name]
[Same pattern — flowchart when 3+ steps or branching, else brief prose only.]
```

---

### Tutorial

```markdown
# [Outcome the learner achieves]

## Overview
What the learner will build or accomplish. Why it matters.
Estimated time: X minutes.

## Prerequisites
- [Specific requirement with version if applicable]
- [Access or permission required]

## Step 1: [First milestone]
[Exact command or action.]
Expected result: [What should the learner see or have?]

## Step 2: [Next milestone]
[Exact command or action.]
Expected result: [Observable confirmation.]

...

## What you learned
- [Concept 1]
- [Concept 2]

## Next steps
- [Link to related how-to guide]
- [Link to reference page]
```

### How-to Guide

```markdown
# How to [accomplish specific goal]

## Before you begin
- [Required tool or access, with setup link if non-trivial]
- [Pre-condition that must be true]

## Flow

\`\`\`mermaid
flowchart TD
  start[Start] --> checkPrecond{Precondition met?}
  checkPrecond -->|Yes| stepOne[Step 1 action]
  checkPrecond -->|No| fixPrecond[Fix precondition]
  fixPrecond --> stepOne
  stepOne --> stepTwo[Step 2 action]
  stepTwo --> done[Done]
\`\`\`

[Max 3 sentences caption. Do not restate the diagram.]

## Steps
1. [Exact action] → Expected: [What you should see]
2. [Exact action] → Expected: [What you should see]
3. ...

## Verify it worked
[Specific command or observation that proves success, not just "check the UI".]
```

### Reference

```markdown
# [Component / Command / Config] Reference

## Overview
[One-paragraph description of what this component or command does.]

## [Parameters / Config Keys / Options]
| Name | Type | Required | Default | Description |
|---|---|---|---|---|
| [exact-key-name] | [type] | Yes/No | [actual default from source] | [Effect on behavior] |
| ... | ... | ... | ... | ... |

## Examples

### Minimal
\`\`\`
[Smallest valid usage]
\`\`\`

### Full
\`\`\`
[Usage with all significant options set]
\`\`\`

## Related
- [Link to how-to guide that uses this]
- [Link to explanation if design rationale is relevant]
```

### Explanation

```markdown
# Understanding [concept]

## Background
[The problem or situation this concept exists to address.]

## How it works

\`\`\`mermaid
flowchart TD
  trigger[Trigger] --> process[Core process]
  process --> outcome[Outcome]
\`\`\`

[Max 3 sentences. Mental model only — details not already in the diagram.]

## Design trade-offs
[What was chosen and why. What was rejected and why.]

## Further reading
- [Link to reference page]
- [Link to ADR if design decision is relevant]
```

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
- Sequential logic described in long prose without a Mermaid flowchart (3+ steps or branching)
- Prose under a diagram that restates nodes or edges already shown
- Diagram nodes or edges not verifiable from source material

---

## Verification Checklist

Run this checklist for user-track content, then run the **skill-level VERIFICATION checklist** in `SKILL.md` before delivering any output.

- [ ] `user-playbook.md` leads with a Core Features & Component Overview section
- [ ] System overview Mermaid flowchart present when project has 2+ components or multi-step user flow
- [ ] Every 3+ step or branching process opens with a Mermaid flowchart before prose
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
