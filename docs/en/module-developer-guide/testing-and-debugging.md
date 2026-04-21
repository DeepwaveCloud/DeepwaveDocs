# Testing and Debugging

## Testing strategy

Every module should be verified at three levels.

### Unit tests

Validate parameter handling, pure logic, boundary conditions, and deterministic outputs.

### Contract tests

Verify the module behaves correctly against the public platform contract:

- correct `type`,
- valid metadata,
- expected output structure,
- safe handling of missing context values,
- no forbidden side effects in normal execution.

### Composition tests

Run the module inside a realistic module chain with upstream and downstream dependencies represented by test doubles or lightweight reference modules.

## Recommended pytest structure

```text
tests/
  test_params.py
  test_lifecycle.py
  test_outputs.py
  test_composition.py
```

## Debugging guidance

- Inspect module inputs before changing business logic.
- Confirm upstream module identifiers match configuration references.
- Check ordering and runtime mode assumptions first.
- Reproduce with the smallest valid composition.
- Record module output shape changes in docs and tests together.

## Failure triage table

| Symptom | Likely cause | First check |
| --- | --- | --- |
| Module fails at creation | Invalid params | Schema and semantic validation |
| Output missing downstream | Wrong module id or missing context write | `module_id` and output mapping |
| Composition rejected | Ordering or required type violation | Validation rules |
| Behavior differs by mode | Unsupported execution assumption | Runtime compatibility note |

## Logging guidance

Prefer structured, contextual diagnostics in the host application. A module should expose enough state for diagnosis without coupling itself to a specific logging backend.

## Release gate

Do not publish a module unless its tests, examples, and documentation agree on the same parameter names, outputs, and compatibility assumptions.
