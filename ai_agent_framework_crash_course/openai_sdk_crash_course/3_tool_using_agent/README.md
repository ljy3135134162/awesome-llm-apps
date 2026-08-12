# 🎯 教程 3：工具调用 Agent

本教程进入 Agent 工具系统，学习如何让 Agent 使用**自定义函数和内置工具**执行具体任务。工具让 Agent 不再局限于生成文本，而能够执行现实世界中的操作。

## 🎯 你将学到什么

- **函数工具**：将自定义 Python 函数创建为 Agent 工具
- **内置工具**：使用 OpenAI 提供的预构建能力
- **工具集成**：有效地为 Agent 配置工具
- **工具执行**：理解 Agent 如何判断何时调用工具

## 🧠 核心概念：OpenAI Agents SDK 中的工具

工具是**Agent 可以调用的函数或能力**，可以理解为 Agent 的“手”。通过工具，Agent 可以：
- 执行计算和数据处理
- 搜索 Web 并获取实时信息
- 执行代码和分析数据
- 调用外部 API 与服务
- 访问数据库和文件系统

```text
┌─────────────────────────────────────────────────────────────┐
│                       带工具的 Agent                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐     │
│  │    输入     │───▶│    Agent    │───▶│    输出     │     │
│  │ “计算复利”  │    │ 推理+工具调用│    │ “计算结果…” │     │
│  └─────────────┘    └─────────────┘    └─────────────┘     │
│                             │                               │
│                             ▼                               │
│                      ┌─────────────┐                       │
│                      │    工具     │                       │
│                      │  Calculator │                       │
│                      │  Web Search │                       │
│                      │  File I/O   │                       │
│                      └─────────────┘                       │
└─────────────────────────────────────────────────────────────┘
```

## 🔧 工具类型

### 1. **Function Tools**
将自己编写的 Python 函数暴露给 Agent：

```python
@function_tool
def calculate_compound_interest(principal: float, rate: float, time: int) -> float:
    """Calculate compound interest"""
    return principal * (1 + rate) ** time
```

### 2. **内置工具**
OpenAI 提供多种预构建工具，例如：
- **WebSearchTool**：搜索 Web 获取最新信息
- **CodeInterpreterTool**：执行代码并进行计算或数据分析
- **FileSearchTool**：搜索已上传文件中的内容

## 🚀 教程概览

本教程包含三个工具集成示例：

### **1. Function Tools**（`3_1_function_tools/`）
- 将自定义 Python 函数作为工具
- `@function_tool` 装饰器
- 基础数学与实用函数

### **2. Built-in Tools**（`3_2_builtin_tools/`）
- 集成 OpenAI WebSearchTool
- 使用 CodeInterpreterTool 执行计算
- 使用预构建工具能力

### **3. Agents as Tools**（`3_3_agents_as_tools/`）
- 将 Agent 作为另一个 Agent 的工具
- 专业 Agent 协作
- 高级 Agent 组合模式

## 📁 项目结构

```text
3_tool_using_agent/
├── README.md                    # 本文件：概念说明
├── requirements.txt             # 依赖
├── 3_1_function_tools/          # 自定义函数工具
│   ├── __init__.py
│   ├── tools.py                 # 自定义工具定义
│   └── agent.py                 # 使用函数工具的 Agent
├── 3_2_builtin_tools/           # 内置工具集成
│   ├── __init__.py
│   └── agent.py                 # 使用内置工具的 Agent
├── 3_3_agents_as_tools/         # Agents as Tools 模式
│   ├── __init__.py
│   ├── agent.py                 # 基础 Agent 编排
│   └── advanced_agent.py        # 带 Runner 配置的高级 Agent 工具
├── app.py                       # Streamlit Web 界面（可选）
└── env.example                  # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何使用 `@function_tool` 创建自定义函数工具
- ✅ 如何集成 WebSearch、CodeInterpreter 等内置工具
- ✅ Agent 如何判断何时以及如何使用工具
- ✅ 工具设计与集成的最佳实践
- ✅ 工具错误处理和参数验证

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

3. **配置环境变量**：
   ```bash
   cp env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

4. **测试 Calculator Agent**：
   ```bash
   python calculator_agent.py
   ```

5. **测试 Research Agent**：
   ```bash
   python research_agent.py
   ```

6. **测试 Data Analysis Agent**：
   ```bash
   python data_analysis_agent.py
   ```

7. **运行交互式 Web 界面**：
   ```bash
   streamlit run app.py
   ```

## 🧪 示例场景

### Agents as Tools
可以尝试：
- `Translate 'Hello, how are you?' to Spanish and French`
- `Say 'Good morning' in all available languages`
- `Research artificial intelligence and write a professional summary`

### Research Agent
可以尝试：
- `What's the latest news about artificial intelligence?`
- `Find information about renewable energy trends`
- `Search for Python programming best practices`

### Data Analysis Agent
可以尝试：
- `Analyze this CSV data: [paste some data]`
- `Create a simple bar chart of sales data`
- `Calculate statistical measures for this dataset`

## 🔧 常见工具模式

### 1. **简单函数工具**
```python
@function_tool
def add_numbers(a: float, b: float) -> float:
    """Add two numbers together"""
    return a + b
```

### 2. **带验证的复杂函数工具**
```python
@function_tool
def get_weather(city: str, units: str = "metric") -> str:
    """Get current weather for a city"""
    if not city.strip():
        return "Error: City name cannot be empty"

    # API call logic here
    return f"Weather data for {city}"
```

### 3. **Agents as Tools 集成**
```python
from agents import Agent

translator = Agent(
    name="Translator",
    instructions="Translate text"
)

orchestrator = Agent(
    name="Orchestrator",
    instructions="Coordinate translation tasks",
    tools=[
        translator.as_tool(
            tool_name="translate_text",
            tool_description="Translate user's message"
        )
    ]
)
```

## 💡 工具设计最佳实践

- **清晰的 Docstring**：Agent 依赖工具描述理解工具用途
- **完整的类型标注**：为参数和返回值提供准确的 Type Hint
- **错误处理**：捕获异常并返回有意义的信息
- **简单参数设计**：工具参数应明确且尽量精简
- **单一职责**：每个工具专注完成一类任务

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 4：Agent 运行方式](../4_running_agents/README.md)** —— 掌握不同执行模式
- **[教程 5：上下文管理](../5_context_management/README.md)** —— 管理多轮交互中的状态
- **[教程 6：Guardrail 与验证](../6_guardrails_validation/README.md)** —— 添加安全与验证机制

## 🚨 故障排查

- **工具没有被调用**：检查工具 Docstring 是否清晰描述了用途
- **类型错误**：确认参数类型与函数签名一致
- **Import 问题**：确认已经导入 `function_tool`
- **API 错误**：使用内置工具时检查 OpenAI API Key 和相关权限
