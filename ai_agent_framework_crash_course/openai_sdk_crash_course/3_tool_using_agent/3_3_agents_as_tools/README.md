# Agents as Tools

本示例演示一种更高级的编排模式：把 Agent 本身作为工具，供其他 Agent 调用。

## 🎯 本示例展示的内容

- **Agent.as_tool()**：将 Agent 转换成可被其他 Agent 调用的工具
- **自定义 Agent 工具**：结合 `@function_tool` 与 `Runner.run()`
- **多 Agent 工作流**：协调多个专业 Agent 完成复杂任务
- **独立配置**：为不同 Agent 设置各自的 `max_turns`、`run_config` 等参数

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

3. **运行基础编排示例**：
   ```python
   from agents import Runner
   from agent import root_agent

   result = Runner.run_sync(root_agent, "Say 'Hello, how are you?' in Spanish.")
   print(result.final_output)
   ```

4. **尝试高级编排示例**：
   ```python
   from advanced_agent import advanced_orchestrator

   result = Runner.run_sync(
       advanced_orchestrator,
       "Research the benefits of AI in healthcare."
   )
   print(result.final_output)
   ```

## 💡 核心概念

### 基础 Agent 工具（`agent.py`）
- **Agent.as_tool()**：最直接的 Agent → Tool 转换方式
- **翻译编排**：由多个语言 Agent 协同处理任务
- **工具命名**：为 Agent 工具设置清晰的名称和描述

### 高级 Agent 工具（`advanced_agent.py`）
- **`@function_tool` + `Runner.run()`**：手动封装更灵活的 Agent 工具
- **独立运行配置**：针对每次调用配置 `max_turns`、temperature 等参数
- **研究 + 写作流水线**：把复杂任务拆成多个阶段，由不同 Agent 完成

## 🧪 可用模式

### 基础编排
- Spanish Translation Agent
- French Translation Agent
- Orchestrator 负责协调不同语言任务

### 高级编排
- Research Agent 负责信息收集
- Writing Agent 负责内容生成
- 使用自定义工具函数封装 Runner 配置

## 🧠 为什么要把 Agent 当作工具？

这种模式适合把复杂系统拆成多个职责清晰的专业 Agent。例如：

```text
用户请求
   │
   ▼
Orchestrator Agent
   ├──▶ Research Agent
   ├──▶ Translation Agent
   └──▶ Writing Agent
```

与直接 Handoff 不同，`Agent.as_tool()` 更适合“主 Agent 保持控制权，但按需调用专业 Agent”的架构。主 Agent 可以接收子 Agent 的结果后继续推理、组合结果，或者再调用其他工具。

## 🔧 基础模式

```python
translator = Agent(
    name="Spanish Translator",
    instructions="Translate the user's text into Spanish."
)

root_agent = Agent(
    name="Orchestrator",
    instructions="Use the translation tools when needed.",
    tools=[
        translator.as_tool(
            tool_name="translate_to_spanish",
            tool_description="Translate text into Spanish"
        )
    ]
)
```

这里，`root_agent` 始终是当前主 Agent。它只是把翻译任务委托给另一个 Agent，并拿回结果。

## 🔧 高级模式

如果你需要对子 Agent 的执行方式进行更精细的控制，可以使用自定义 Function Tool：

```python
@function_tool
async def research_topic(topic: str) -> str:
    result = await Runner.run(
        research_agent,
        topic,
        max_turns=5
    )
    return result.final_output
```

这种方式适合：
- 为不同子 Agent 设置不同的运行参数
- 控制最大轮次
- 在工具函数内加入日志、验证或错误处理
- 在返回主 Agent 前对结果进行二次加工

## 📊 Agent as Tool 与 Handoff 的区别

| 模式 | 控制权 | 适合场景 |
|---|---|---|
| **Agent as Tool** | 主 Agent 保持控制 | 调用专业能力、组合多个结果 |
| **Handoff** | 控制权转移给另一个 Agent | 客服转接、专业角色接管对话 |

如果任务是“帮我查资料，然后继续帮我写总结”，通常更适合 Agents as Tools。

如果任务是“这个问题应该由账单客服继续和用户沟通”，通常更适合 Handoff。

## 💡 最佳实践

- **职责单一**：每个子 Agent 只负责一个清晰领域
- **工具描述明确**：让主 Agent 容易判断何时调用哪个 Agent
- **限制递归调用**：避免 Agent 相互调用形成无限循环
- **控制成本**：多 Agent 编排可能显著增加模型调用次数
- **明确返回值**：子 Agent 应输出主 Agent 容易继续使用的结果

## 🔗 后续步骤

- [Function Tools](../3_1_function_tools/README.md) —— 自定义函数工具
- [Built-in Tools](../3_2_builtin_tools/README.md) —— SDK 内置工具
- [教程 4：运行 Agent](../../4_running_agents/README.md) —— 更高级的执行模式
