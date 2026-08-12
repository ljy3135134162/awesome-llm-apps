# 高级 Handoff

本示例展示 OpenAI Agents SDK 中更高级的 Handoff 配置，包括回调函数、结构化输入以及自定义工具名称。

## 🎯 本示例展示的内容

- **自定义 Handoff 配置**：使用 `handoff()` 函数配置任务移交
- **回调函数**：在 Handoff 被触发时执行自定义代码
- **结构化输入数据**：在 Handoff 时传递明确的数据结构
- **工具自定义**：自定义 Handoff 工具的名称和描述

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Agent**：
   ```python
   import asyncio
   from agent import main

   # 测试高级 Handoff
   asyncio.run(main())
   ```

## 💡 核心概念

- **`handoff()` 函数**：对 Handoff 行为进行自定义配置
- **回调执行**：通过 `on_handoff` 函数跟踪和处理任务移交事件
- **结构化输入**：使用 Pydantic 模型定义 Handoff 数据
- **工具覆盖配置**：自定义工具名称和描述

## 🧪 高级功能

### 使用结构化数据进行升级处理

```python
class EscalationData(BaseModel):
    reason: str
    priority: str
    customer_id: str

escalation_handoff = handoff(
    agent=escalation_agent,
    tool_name_override="escalate_to_manager",
    input_type=EscalationData
)
```

这里通过 `EscalationData` 明确定义升级请求需要携带的原因、优先级和客户 ID，使接收任务的 Agent 能获得稳定、可验证的数据。

### 回调函数

```python
async def on_escalation_handoff(ctx, input_data):
    print(f"🚨 ESCALATION: {input_data.reason}")
    # 记录到监控系统
    # 发送通知
    # 更新工单
```

Handoff 发生时可以执行额外逻辑，例如记录日志、发送告警、更新工单系统或统计指标。

### 自定义工具配置

- **`tool_name_override`**：覆盖自动生成的工具名称
- **`tool_description_override`**：覆盖工具描述
- **`input_filter`**：控制哪些上下文会传递给目标 Agent
- **`is_enabled`**：动态启用或禁用某个 Handoff

## 💻 高级模式

### 升级 Handoff

适合处理：
- 愤怒或需要人工升级的客户场景
- 高金额退款请求
- 复杂技术问题

### 回调集成

Handoff 回调可以用于：
- 日志记录与监控
- 通知系统
- 指标统计与追踪

### 输入过滤

通过输入过滤可以实现：
- 清理无关上下文
- 移除敏感数据
- 对传递给下一个 Agent 的对话内容进行净化

## 🧠 为什么需要高级 Handoff

基础 Handoff 适合简单的 Agent 路由，但生产环境通常还需要控制**什么时候允许移交、移交什么数据，以及移交发生后执行什么操作**。

高级 Handoff 将任务委派从单纯的 Agent 跳转扩展为一个可控制的工作流节点：

```text
用户请求
   │
   ▼
Triage Agent
   │
   ├── 判断是否需要 Handoff
   │
   ▼
结构化 Handoff 输入
   │
   ├── input_filter 清理上下文
   ├── on_handoff 执行回调
   │
   ▼
目标 Agent
```

这种模式更适合客服升级、审批、专业 Agent 分流以及需要审计记录的多 Agent 系统。

## 🔗 后续步骤

- [基础 Handoff](../8_1_basic_handoffs/README.md) —— Handoff 基础概念
- [教程 9：多 Agent 编排](../../9_multi_agent_orchestration/README.md) —— 构建更复杂的多 Agent 工作流
