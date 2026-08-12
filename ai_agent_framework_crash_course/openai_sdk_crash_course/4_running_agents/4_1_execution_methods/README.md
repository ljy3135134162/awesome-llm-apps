# Agent 执行方式

本示例展示 OpenAI Agents SDK 提供的三种 Agent 执行方式：异步、同步和流式执行。

## 🎯 本示例展示的内容

- **`Runner.run()`**：用于非阻塞操作的异步执行
- **`Runner.run_sync()`**：用于简单阻塞调用的同步执行
- **`Runner.run_streamed()`**：用于实时响应的流式执行
- **执行方式对比**：根据应用场景选择合适的运行模式

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境变量**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Agent**：
   ```python
   from agents import Runner
   from agent import root_agent

   # 测试同步执行
   result = root_agent.sync_execution_example()
   print(result)
   ```

## 💡 核心概念

- **同步执行（Sync Execution）**：阻塞当前线程直到 Agent 完成，调用方式最简单
- **异步执行（Async Execution）**：不会阻塞事件循环，适合并发任务和异步应用
- **流式执行（Streaming Execution）**：在完整结果生成之前持续接收事件和响应内容
- **场景选择**：应根据应用的并发需求、延迟要求和交互方式选择执行模式

## 🔍 三种执行方式的区别

| 执行方式 | API | 特点 | 典型场景 |
|---|---|---|---|
| 同步 | `Runner.run_sync()` | 调用简单，但会阻塞直到完成 | CLI、脚本、简单后端任务 |
| 异步 | `Runner.run()` | 支持 `await`，适合并发执行 | FastAPI、异步服务、多任务处理 |
| 流式 | `Runner.run_streamed()` | 可以边生成边消费事件 | 聊天 UI、实时输出、长响应 |

### 同步执行

```python
result = Runner.run_sync(agent, "分析这个问题")
print(result.final_output)
```

同步模式最适合不需要并发控制的简单程序。调用返回时，Agent 的整个运行过程已经结束。

### 异步执行

```python
result = await Runner.run(agent, "分析这个问题")
print(result.final_output)
```

异步模式允许 Python 事件循环在等待模型或工具调用时继续处理其他任务，因此更适合服务端和高并发应用。

### 流式执行

```python
result = Runner.run_streamed(agent, "分析这个问题")

async for event in result.stream_events():
    print(event)
```

流式模式不仅可以用于逐步展示模型输出，还可以监听 Agent 运行过程中产生的工具调用、Agent 切换等事件。

## 🧠 如何选择

- 如果只是运行一个简单脚本，优先考虑 **`run_sync()`**。
- 如果应用本身基于 `asyncio`，优先使用 **`run()`**。
- 如果用户需要实时看到生成过程，使用 **`run_streamed()`**。
- 如果需要同时运行多个 Agent 或任务，异步执行通常比同步执行更合适。

## 🔗 后续步骤

- [对话管理](../4_2_conversation_management/README.md) —— 管理多轮对话和会话状态
- [运行配置](../4_3_run_configuration/README.md) —— 学习高级运行参数
- [流式事件](../4_4_streaming_events/README.md) —— 深入处理实时 Agent 事件
