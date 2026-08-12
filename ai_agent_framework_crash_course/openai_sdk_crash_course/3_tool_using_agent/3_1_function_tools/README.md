# Function Tools Agent

本示例演示如何使用 `@function_tool` 装饰器创建自定义函数工具，并将普通 Python 函数暴露给 OpenAI Agent 调用。

## 🎯 本示例展示的内容

- **自定义函数工具**：使用 `@function_tool` 创建 Agent 工具
- **工具描述**：通过清晰的 Docstring 帮助 LLM 理解工具用途
- **参数处理**：使用类型标注和默认参数定义工具输入
- **错误处理**：在工具执行失败时提供可控、易理解的返回结果

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

   result = Runner.run_sync(root_agent, "What time is it in New York?")
   print(result.final_output)
   ```

## 💡 核心概念

- **`@function_tool` 装饰器**：将 Python 函数转换为 Agent 可调用的工具
- **Tool Docstring**：LLM 根据函数说明判断何时应该使用工具
- **Type Hint**：用于参数说明、Schema 生成和输入验证
- **工具注册**：将函数工具加入 Agent 的 `tools` 配置

## 🧪 可用工具

### `get_current_time(timezone: str = "UTC")`
- 返回指定时区的当前时间
- 包含时区校验和错误处理逻辑

### `greet_user(name: str)`
- 简单的用户问候工具
- 演示 LLM 如何向函数参数传递值

## 🔗 后续步骤

- [Built-in Tools](../3_2_builtin_tools/README.md) —— 使用 WebSearch、CodeInterpreter 等内置工具
- [Agents as Tools](../3_3_agents_as_tools/README.md) —— 学习更高级的 Agent 编排模式
