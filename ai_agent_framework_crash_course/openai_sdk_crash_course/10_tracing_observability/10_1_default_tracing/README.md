# 默认 Tracing

本示例展示 OpenAI Agents SDK 内置的自动 Tracing 系统。无需额外配置，Agent 工作流中的关键事件就会被自动记录。

## 🎯 本示例展示的内容

- **自动 Tracing**：默认启用的工作流监控能力
- **Trace ID**：为每次 Agent 运行提供唯一标识
- **OpenAI Dashboard**：用于查看和分析 Trace 的可视化平台
- **Tracing 配置**：按需启用或关闭 Trace 采集

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

   # 测试默认 Tracing（自动启用）
   asyncio.run(main())
   ```

## 💡 核心概念

- **零额外配置**：Tracing 默认即可工作
- **唯一运行标识**：每次 `Runner.run()` 都会关联独立的 Trace 标识
- **自动采集**：记录模型调用、工具执行和性能相关信息
- **可视化分析**：可以在 OpenAI 平台中查看工作流执行详情

## 🧪 自动采集内容

### 默认会记录什么

- **LLM Generation**：输入 Prompt、模型响应以及 Token 使用情况
- **Tool Call**：工具执行、参数和返回结果
- **Handoff**：Agent 之间的任务移交
- **Performance**：执行时间和延迟信息
- **Error**：异常与失败信息

### Trace 信息

```python
result = await Runner.run(agent, "Hello")
print(f"Trace ID: {result.run_id}")
# 每次运行都会获得用于定位对应 Trace 的唯一标识
```

### 独立 Trace

- 每次 `Runner.run()` 调用通常对应一个独立 Trace
- 多次运行会产生多个独立 Trace
- 不同工作流可以分别进行观察和排查

## 💻 Tracing 示例

### 基础自动 Tracing

```python
# 默认自动记录，无需额外配置
result = await Runner.run(agent, "Explain machine learning")
print(f"View trace: https://platform.openai.com/traces/{result.run_id}")
```

### Tracing 配置

```python
# 对特定运行关闭 Tracing
result = await Runner.run(
    agent,
    "Private conversation",
    run_config=RunConfig(tracing_disabled=True)
)
```

### 多个 Trace

```python
# 每次运行都会形成独立的 Trace
result1 = await Runner.run(agent, "Question 1")  # Trace 1
result2 = await Runner.run(agent, "Question 2")  # Trace 2
```

## 🔍 Dashboard 能力

### OpenAI Traces Dashboard

可以用于查看：
- **工作流时间线**：观察 Agent、工具和模型调用的执行顺序
- **性能指标**：分析响应时间和 Token 使用情况
- **错误追踪**：定位异常和失败环节
- **输入输出检查**：查看工作流各阶段的数据

### 使用特点

- 通常不需要额外搭建独立的 Tracing 服务
- 使用 OpenAI API 的工作流可以直接集成
- Trace 可用于实时调试以及后续问题分析
- 对复杂多 Agent 工作流尤其有价值

## 🧠 为什么 Tracing 很重要

当 Agent 系统开始包含工具调用、Handoff 和多 Agent 编排后，仅查看最终输出往往无法判断问题出在哪里。Tracing 可以把一次运行拆解为可观察的执行链：

```text
用户输入
   │
   ▼
Agent 推理
   │
   ├── LLM Call
   ├── Tool Call
   ├── Handoff
   └── Final Output

每个阶段都会留下可观测信息
```

这使开发者能够判断问题究竟来自 Prompt、模型、工具、任务路由还是执行性能。

## 🔗 后续步骤

- [自定义 Tracing](../10_2_custom_tracing/README.md) —— 学习更高级的 Trace 组织和自定义方式
- [教程 11：Voice](../../11_voice/README.md) —— 继续学习语音 Agent 工作流
