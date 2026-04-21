# 配置与 DSL

## 配置哲学

平台采用类型化、可校验、配置驱动的组合方式。模块作者应暴露稳定参数，并将环境细节与模块逻辑分离。

## 配置层次

### 模块参数层

每个模块通过 `params_schema` 和语义校验函数定义自己的参数契约。

### 平台模块配置层

平台整体还定义了用于共享模块设置和策略装配的类型化配置模型。模块作者应尽量与这些既有约定在命名、默认值和数据类型上保持一致，但不应依赖内部子系统细节。

## 策略组合示例

```yaml
strategy:
  name: trend_following_reference
  modules:
    - type: indicator
      name: moving_average_fast
      params:
        lookback: 20
    - type: indicator
      name: moving_average_slow
      params:
        lookback: 50
    - type: signal
      name: crossover_signal
      params:
        fast_ref: moving_average_fast
        slow_ref: moving_average_slow
    - type: risk
      name: volatility_guard
      params:
        max_volatility: 0.18
    - type: position
      name: directional_position
      params:
        mode: long_short
    - type: execution
      name: simulated_execution
      params:
        timeout_ms: 5000
```

## 参数设计规则

- 参数名要明确；
- 数值参数尽量有边界；
- 优先使用简单类型和小型结构；
- 调优参数与环境配置分离；
- 默认值和失败条件必须写清楚。

## 参数表模板

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `lookback` | integer | 是 | 使用的历史观测数量 |
| `threshold` | float | 否 | 决策灵敏度，通常应有边界 |
| `mode` | string | 否 | 命名化运行模式 |

## DSL 编写建议

如果团队维护自己的策略 DSL 或配置生成器，模块面对开发者的部分应保持稳定、声明式。模块应能在不知道任何内部基础设施实现的前提下完成配置。

## 配置反模式

- 在模块示例中写入主机、密钥或凭据；
- 一个参数承担多个含义；
- 依赖未写入文档的隐式默认值；
- 把内部适配器参数暴露为公开模块参数。
