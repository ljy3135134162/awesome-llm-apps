# 内置工具 Agent

本示例演示如何使用 OpenAI Agents SDK 提供的内置工具，例如 `WebSearchTool` 和 `CodeInterpreterTool`。

## 🎯 本示例展示的内容

- **WebSearchTool**：为 Agent 提供实时 Web 搜索能力
- **CodeInterpreterTool**：执行代码、数学计算和数据处理
- **内置工具集成**：直接使用 SDK 已提供的工具能力
- **多工具组合**：在同一个 Agent 中同时使用多个不同类型的工具

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

   result = Runner.run_sync(
       root_agent,
       "What's the latest news about AI and calculate 15% of 200?"
   )
   print(result.final_output)
   ```

## 💡 关键概念

- **WebSearchTool()**：搜索 Web，获取需要实时信息的问题答案
- **CodeInterpreterTool()**：执行 Python 代码、计算和数据分析
- **工具实例化**：使用默认配置直接创建 SDK 内置工具实例
- **多工具 Agent**：将不同工具组合到同一个 Agent 中，由模型根据任务自行选择

## 🧪 可用工具

### WebSearchTool

- 搜索互联网中的最新信息
- 适合回答依赖当前数据或近期事件的问题
- 搜索结果会自动整理为 Agent 可使用的上下文

### CodeInterpreterTool

- 在受控环境中执行 Python 代码
- 适合数学计算和程序化处理
- 可用于数据分析以及更复杂的计算任务

## 🔗 后续步骤

- [Function Tools](../3_1_function_tools/README.md) —— 学习创建自定义函数工具
- [Agents as Tools](../3_3_agents_as_tools/README.md) —— 学习更高级的 Agent 编排模式
