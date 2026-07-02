# ASCII Diagram Guide

Use ASCII flowcharts in fenced `text` blocks for workflows in `user-playbook.md` and `developer-playbook.md`. This format renders on Jekyll and other static-site generators without plugins — unlike Mermaid, which requires JavaScript or a build plugin.

Do not use Mermaid in skill output. Do not use diagrams in `README.md`.

---

## When required

Same trigger as the skill-level diagram policy:

- Any workflow, pipeline, request path, component interaction, or decision logic with **3+ steps or any branching** must open with an ASCII flowchart before prose.
- System overview when the project has 2+ components or a multi-step user flow.
- Module-level and per-function flows for pipeline stages and orchestrators.

Prose follows the diagram — never the reverse. Max ~3 sentences under each diagram.

---

## Fence and placement

- Fence language: ` ```text ` (not `mermaid`, not bare fences)
- Location: `user-playbook.md` and `developer-playbook.md` only
- Width: max ~72 characters per line — readable on mobile and narrow Jekyll layouts

---

## Syntax

| Element | Pattern |
|---------|---------|
| Linear step | `[Label A] --> [Label B] --> [Label C]` |
| Vertical flow | Stack steps on separate lines with `v` or `-->` between rows |
| Decision | `[Condition?]` with branching arms |
| Yes branch | `\|-- Yes --> [Next step]` |
| No branch | `+-- No  --> [Alternate step]` |
| Loop back | `--+` at end of a branch line returning to an earlier node |

Use `[Label]` for nodes. Labels must match verified source behavior — same anti-invention rules as prose and tables.

---

## Patterns

### Left-to-right (system overview)

```text
[User input] --> [Component A] --> [Component B] --> [User output]
```

### Top-down (component or pipeline)

```text
[Start]
   |
   v
[Step one] --> [Step two] --> [Result]
```

### Branching (how-to decision path)

```text
[Start] --> [Precondition met?]
              |-- Yes --> [Step 1] --> [Step 2] --> [Done]
              +-- No  --> [Fix precondition] --+
```

### Module flow (developer reference)

```text
[Input] --> [stage_one] --> [stage_two] --> [Output]
```

### Add-a-feature workflow

```text
[Create module] --> [Register config] --> [Wire pipeline]
       --> [Write tests] --> [Deploy and validate]
```

---

## Good vs bad

**Good** — verified labels, under 72 chars, branching is explicit:

```text
[Request] --> [Auth check?]
                |-- Yes --> [Handler] --> [Response]
                +-- No  --> [401 Unauthorized]
```

**Bad** — invented node not in source:

```text
[Request] --> [Magic cache layer] --> [Response]
```

**Bad** — restated in long prose under the diagram (use ≤3 sentences instead):

> First the request arrives, then auth is checked, and if auth passes the handler runs...

**Bad** — too wide for Jekyll/mobile:

```text
[Very long component name that describes everything] --> [Another extremely long label that wraps badly on small screens]
```

Split into stacked rows or shorten labels to essential terms.

---

## No invention

Every node, edge, and label must map to verified source behavior. If a branch is unknown, ask in the consolidated gap batch — do not invent a node.

---

## Related

- Output diagram policy: [SKILL.md](../SKILL.md) — OUTPUT FORMAT → Diagrams
- User templates: [templates/user/](../templates/user/)
- Developer templates: [templates/developer/](../templates/developer/)
