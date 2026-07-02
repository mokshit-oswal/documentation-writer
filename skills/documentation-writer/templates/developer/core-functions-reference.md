# [Module Name] — Function Reference

## Module flow

```text
[Input] --> [stage_one] --> [stage_two] --> [Output]
```

[Max 3 sentences. Required when module has pipeline stages or 3+ linked functions.]

---

## function_name

`function_name(param1: Type, param2: Type = default) -> ReturnType`

[One-sentence description of what this function does.]

### Flow
[Include when this function is a pipeline stage or orchestrator with 3+ steps or branching.]

```text
[Consume input] --> [Transform] --> [Produce output]
```

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
```
result = function_name(param1=value, param2=other_value)
```

---
[Repeat for next function]
