# 对话管理

本示例展示 OpenAI Agents SDK 中两种多轮对话管理方式：使用 `to_input_list()` 手动维护对话历史，以及使用 Session 自动管理上下文。

## 🎯 本示例展示的内容

- **手动对话串联**：使用 `result.to_input_list()` 维护历史消息
- **自动 Session 管理**：使用 `SQLiteSession` 管理会话记忆
- **对话上下文**：在多轮交互之间保留状态
- **会话流程管理**：比较手动与自动两种管理方式

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
   import asyncio
   from agent import manual_conversation_example, session_conversation_example

   # 测试手动对话管理
   asyncio.run(manual_conversation_example())
   ```

## 💡 核心概念

- **`to_input_list()`**：将一次 Agent 运行结果转换为下一轮可继续使用的输入历史
- **`SQLiteSession`**：自动保存和恢复多轮对话状态
- **上下文保留**：让 Agent 在后续轮次中理解前面的内容
- **Session 存储**：根据场景选择临时或持久化的会话存储方式

## 🧠 手动管理对话

手动模式下，应用需要显式保存上一轮输入和输出，并将它们作为下一轮输入的一部分继续传递。

```python
result = await Runner.run(agent, "My name is Alice")

history = result.to_input_list()
history.append({
    "role": "user",
    "content": "What is my name?"
})

result = await Runner.run(agent, history)
print(result.final_output)
```

这种方式的优点是上下文完全由应用控制，适合需要自定义裁剪、过滤或重写历史消息的系统。

## 🗃️ 使用 Session 自动管理

使用 Session 时，不需要每次手动重新组装历史消息。SDK 会根据 Session 保存之前的交互，并在后续运行时自动恢复。

```python
session = SQLiteSession("conversation_123")

await Runner.run(
    agent,
    "My name is Alice",
    session=session,
)

result = await Runner.run(
    agent,
    "What is my name?",
    session=session,
)
```

只要继续使用相同的 Session，Agent 就可以访问此前的对话上下文。

## 🔍 手动历史与 Session 的区别

| 方式 | 上下文管理 | 优点 | 适合场景 |
|---|---|---|---|
| `to_input_list()` | 应用手动管理 | 控制最细，可自由修改历史 | 自定义工作流、上下文裁剪、特殊消息处理 |
| Session | SDK 自动管理 | 代码简单，天然支持多轮会话 | 聊天应用、长期会话、多用户系统 |

## 🧩 Session ID 的作用

Session ID 用来区分不同的对话线程。例如：

```text
user_001 → session_A → 对话历史 A
user_001 → session_B → 对话历史 B
user_002 → session_C → 对话历史 C
```

因此，同一个用户也可以同时拥有多个独立对话，而不会互相污染上下文。

## 💡 选择建议

- 需要完全控制上下文内容时，使用 **`to_input_list()`**。
- 需要快速构建稳定的多轮聊天体验时，使用 **Session**。
- 多用户应用中，应为不同用户或不同会话分配独立 Session ID。
- 长会话中仍需考虑上下文长度、历史裁剪和摘要策略。

## 🔗 后续步骤

- [执行方式](../4_1_execution_methods/README.md) —— 同步、异步和流式执行
- [运行配置](../4_3_run_configuration/README.md) —— 控制 Agent 运行参数
- [流式事件](../4_4_streaming_events/README.md) —— 实时处理 Agent 运行事件
- [教程 7：Sessions](../../7_sessions/README.md) —— 深入学习 Session 与持久化记忆
