# 模块契约

## 核心要求

每一个自定义模块都必须继承平台模块基类，并提供一致、可预测的元数据和行为接口。

## 必要契约元素

| 元素 | 责任 |
| --- | --- |
| `name` | 模块公开标识符 |
| `version` | 模块版本 |
| `type` | 模块类型声明 |
| `description` | 模块用途说明 |
| `params_schema` | 参数结构定义 |
| `validate_params` | 参数语义校验 |
| `init` | 一次性初始化 |
| `next_async` | 主执行入口 |

## 可选契约元素

| 元素 | 用途 |
| --- | --- |
| `aliases` | 向后兼容或别名能力 |
| `tick_update` | Tick 或 forming bar 更新逻辑 |
| `on_error` | 自定义错误钩子 |
| `get_metrics` | 暴露模块指标 |
| `get_metadata` | 导出元数据 |

!!! warning "受限宿主钩子"
    宿主侧结果处理扩展不属于标准外部模块契约。除非经过明确批准，不要让第三方模块依赖任何未公开的宿主钩子。

## 生命周期概览

1. 构造模块时先做参数校验。
2. 平台为模块分配或解析模块标识。
3. `init` 在正式执行前调用一次。
4. `next_async` 作为主要运行入口被调用。
5. 如果模块支持 Tick 级别更新，运行时可能调用 `tick_update`。
6. 错误在某些场景下会流经 `on_error`。

## 实现建议

### 参数校验要分层

`params_schema` 负责结构层校验，`validate_params` 负责业务语义校验。结构层解决字段、类型、是否必填，语义层解决取值范围、组合关系和业务合法性。

### 初始化要轻量

`init` 应只负责准备模块状态，不应在这里做网络调用、文件写入或重型环境绑定。

### 以异步实现为主

推荐把 `next_async` 作为真实逻辑来源。同步入口主要用于兼容，不应成为首选扩展模型。

## 最小实现模板

```python
from typing import Any, Dict

from src.strategy_modules.core.module_base import BacktestContext, ModuleBase, ModuleType


class ExampleSignal(ModuleBase):
    name = "example_signal"
    version = "1.0.0"
    type = ModuleType.SIGNAL
    description = "参考信号模块"
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

## 每个模块必须附带的文档

- 模块作用与所属类型；
- 参数表；
- 输入假设；
- 输出契约；
- 失败场景；
- 配置示例；
- 兼容性说明。
