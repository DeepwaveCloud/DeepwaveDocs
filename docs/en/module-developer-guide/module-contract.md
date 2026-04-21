# Module Contract

## Core contract

Every custom module must inherit from the platform base contract and supply a predictable set of metadata and behaviors.

## Required contract elements

| Element | Responsibility |
| --- | --- |
| `name` | Public module identifier |
| `version` | Contracted module version |
| `type` | Declared module category |
| `description` | Human-readable purpose |
| `params_schema` | Parameter schema used for validation |
| `validate_params` | Semantic parameter validation |
| `init` | One-time initialization |
| `next_async` | Main execution entrypoint |

## Optional contract elements

| Element | Use |
| --- | --- |
| `aliases` | Backward-compatible or alternate names |
| `tick_update` | Forming-bar or tick-level update logic |
| `on_error` | Custom error handling hook |
| `get_metrics` | Module metrics surface |
| `get_metadata` | Metadata export |

!!! warning "Restricted host hooks"
    Host-side result handling extensions are not part of the standard external module contract. Do not design a third-party module around unpublished host hooks unless your team has explicit approval.

## Lifecycle overview

1. Parameters are validated at construction time.
2. The runtime assigns or resolves the module identifier.
3. `init` is called once before regular execution begins.
4. `next_async` is called for normal module execution.
5. `tick_update` may be called for modules that support incremental updates.
6. Errors may flow through `on_error` if the runtime chooses to surface them there.

## Implementation guidance

### Keep validation layered

Use `params_schema` for structural validation and `validate_params` for semantic validation. A schema should ensure shape and type. Semantic validation should ensure business correctness such as positive lookback windows, bounded thresholds, or valid field relationships.

### Keep initialization light

`init` should prepare module state only. Avoid network calls, file system writes, or heavyweight runtime coupling.

### Prefer async-native logic

Implement `next_async` as the source of truth. The sync wrapper exists for compatibility but should not be treated as the primary extension model.

## Minimal template

```python
from typing import Any, Dict

from src.strategy_modules.core.module_base import BacktestContext, ModuleBase, ModuleType


class ExampleSignal(ModuleBase):
    name = "example_signal"
    version = "1.0.0"
    type = ModuleType.SIGNAL
    description = "Reference signal module"
    params_schema = {
        "lookback": {"type": "int", "required": True},
        "threshold": {"type": "float", "required": True},
    }

    def validate_params(self, params: Dict[str, Any]) -> bool:
        return params["lookback"] > 0 and 0.0 <= params["threshold"] <= 1.0

    def init(self, bt_ctx: BacktestContext) -> None:
        self.last_value = None

    async def next_async(self, bt_ctx: BacktestContext) -> Dict[str, Any]:
        value = bt_ctx.indicators.get("reference_indicator")
        self.last_value = value
        return {"signal": "hold", "value": value}
```

## Documentation expectations for each module

Every module package should ship with:

- module purpose and category,
- parameter table,
- output contract,
- compatibility assumptions,
- example configuration,
- known failure modes.
