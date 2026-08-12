# 🎯 教程 6.3：工具执行回调

## 🎯 你将学到什么
- **工具执行前回调**：监控工具何时开始执行
- **工具执行后回调**：跟踪工具完成状态和结果
- **工具上下文**：理解如何监控工具执行过程

## 🧠 核心概念：工具执行监控

工具执行回调允许你监控 Agent 何时调用工具、跟踪完整执行生命周期并分析结果，从而清楚了解 Agent 如何与外部系统和 API 交互。

### **工具执行流程**
```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Agent 调用工具│───▶│ 工具执行前回调  │───▶│   工具执行      │
│                 │    │                 │    │   （外部系统）  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                              │                       │
                              ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │ 工具执行后回调  │    │   工具结果      │
                       │                 │    │   （返回 Agent）│
                       └─────────────────┘    └─────────────────┘
```

### **回调执行时间线**
```text
时间 → 0ms    5ms    10ms   15ms   20ms   25ms
       │      │      │      │      │      │
       ▼      ▼      ▼      ▼      ▼      ▼
    [调用] [Before] [执行] [After] [结果]
    工具   回调     开始   回调    返回
```

### **应用场景**
- **执行监控**：跟踪工具何时开始和完成
- **参数验证**：执行前检查工具输入
- **结果日志**：记录工具输出和错误
- **调试**：分析工具执行模式
- **数据分析**：统计哪些工具最常被调用

## 🚀 教程概览

本教程将创建一个配置工具执行回调的 Agent，它会：
- 使用一个简单的计算器工具
- 监控工具执行的开始与结束
- 跟踪工具参数和执行结果
- 提供详细的工具使用可见性

## 📁 项目结构

```text
6_3_tool_execution_callbacks/
├── README.md              # 本文件
├── requirements.txt       # 依赖
├── agent.py               # 配置工具回调的 Agent
└── app.py                 # Streamlit 界面
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ **Before Tool Callback**：如何监控工具开始执行
- ✅ **After Tool Callback**：如何跟踪工具执行完成
- ✅ **Tool Context**：如何访问工具和 Agent 信息
- ✅ **FunctionTool**：如何正确注册支持回调的工具
- ✅ **回调集成**：如何将回调与 Agent 组合使用

## 🚀 快速开始

### **配置**
1. **安装依赖**：`pip install -r requirements.txt`
2. **配置环境**：创建 `.env` 并写入 `GOOGLE_API_KEY=your_key`
3. **运行应用**：`streamlit run app.py`

### **测试 Agent**
```bash
# 启动 Streamlit 应用
streamlit run app.py

# 可尝试以下测试消息：
- "Calculate 15 + 27"
- "What is 100 divided by 4?"
- "Multiply 8 by 12"
```

## 🔧 关键概念

### **1. 工具执行前回调**
- **触发时机**：工具开始执行时
- **参数**：`tool`、`args`、`tool_context`
- **用途**：记录工具使用、验证参数、记录开始时间

### **2. 工具执行后回调**
- **触发时机**：工具执行完成时
- **参数**：`tool`、`args`、`tool_context`、`tool_response`
- **用途**：记录结果、处理错误、提供反馈

### **3. Tool Context**
- **Agent 信息**：通过 `tool_context.agent_name` 获取
- **状态管理**：通过 `tool_context.state` 共享数据
- **工具信息**：通过 `tool.name` 获取工具名称

## 🔍 测试示例

### **基础工具调用**
```text
用户："Calculate 15 + 27"

🔧 Tool calculator_tool started
📝 Parameters: {'operation': 'add', 'a': 15.0, 'b': 27.0}
📋 Agent: tool_execution_demo_agent

✅ Tool calculator_tool completed
⏱️ Duration: 0.0012s
📄 Result: 15 + 27 = 42
```

### **错误处理**
```text
用户："What is 10 divided by 0?"

🔧 Tool calculator_tool started
📝 Parameters: {'operation': 'divide', 'a': 10.0, 'b': 0.0}
📋 Agent: tool_execution_demo_agent

✅ Tool calculator_tool completed
⏱️ Duration: 0.0008s
📄 Result: Error: Division by zero
```

## 🎯 各项指标代表什么

### **工具执行前回调输出**
- **🔧 工具名称**：当前正在执行哪个工具
- **📝 参数**：传给工具的输入参数
- **📋 Agent**：哪个 Agent 正在调用工具

### **工具执行后回调输出**
- **✅ 完成状态**：工具执行是否完成
- **⏱️ 持续时间**：工具执行耗时
- **📄 结果**：工具输出或执行结果

## 🎯 关键实现说明

### **必须使用 FunctionTool**
工具必须封装为 `FunctionTool`，回调才能正常工作：

```python
# ✅ 正确：使用 FunctionTool
calculator_function_tool = FunctionTool(func=calculator_tool)
agent = LlmAgent(tools=[calculator_function_tool], ...)

# ❌ 错误：直接传入原始函数不会触发回调
agent = LlmAgent(tools=[calculator_tool], ...)
```

### **回调函数签名**
工具回调必须使用正确的参数顺序：

```python
# ✅ 正确签名
def before_tool_callback(tool: BaseTool, args: dict, tool_context: ToolContext):
    pass

def after_tool_callback(tool: BaseTool, args: dict, tool_context: ToolContext, tool_response: any):
    pass
```

### **事件循环必须执行完成**
不要在收到 `is_final_response()` 后立即中断事件循环：

```python
# ✅ 正确：允许回调完整执行
if event.is_final_response() and event.content:
    response_text = event.content.parts[0].text.strip()
    # 不要 break，让循环自然完成
```

## 🎯 高级模式

### **多个工具**
多个工具可以共用同一组回调：

```python
def weather_tool(city: str) -> str:
    return f"Weather in {city}: Sunny, 25°C"

def calculator_tool(operation: str, a: float, b: float) -> str:
    # ... implementation

# 注册多个工具
weather_function_tool = FunctionTool(func=weather_tool)
calculator_function_tool = FunctionTool(func=calculator_tool)

agent = LlmAgent(
    name="multi_tool_agent",
    model="gemini-3-flash-preview",
    tools=[calculator_function_tool, weather_function_tool],
    before_tool_callback=before_tool_callback,
    after_tool_callback=after_tool_callback
)
```

### **参数验证**
可以在 `before_tool_callback` 中实现验证逻辑：

```python
def before_tool_callback(tool: BaseTool, args: dict, tool_context: ToolContext):
    tool_name = tool.name

    # 验证计算器工具参数
    if tool_name == "calculator_tool":
        if "operation" not in args:
            print("⚠️ Warning: Missing operation parameter")
        if "a" not in args or "b" not in args:
            print("⚠️ Warning: Missing numeric parameters")

    print(f"🔧 Tool {tool_name} started")
    print(f"📝 Parameters: {args}")
    return None
```

### **修改工具结果**
可以在 `after_tool_callback` 中修改返回值：

```python
def after_tool_callback(tool: BaseTool, args: dict, tool_context: ToolContext, tool_response: any):
    tool_name = tool.name

    # 为计算器结果增加上下文信息
    if tool_name == "calculator_tool" and "result" in tool_response:
        operation = args.get("operation", "unknown")
        tool_response["context"] = f"Performed {operation} operation"

    print(f"✅ Tool {tool_name} completed")
    print(f"📄 Result: {tool_response}")
    return tool_response  # 返回修改后的结果
```

## 🔗 后续步骤

完成本教程后，可以继续学习：
- **[高级工具模式](../../4_tool_using_agent/README.md)**：复杂工具架构
- **[自定义工具开发](../../4_tool_using_agent/README.md)**：构建自定义工具
- **[工具集成](../../4_tool_using_agent/README.md)**：集成外部 API

## 📚 相关资源

- [Google ADK 工具回调](https://google.github.io/adk-docs/callbacks/types-of-callbacks/#tool-execution-callbacks)
- [工具开发指南](https://google.github.io/adk-docs/tools/)
- [FunctionTool 文档](https://google.github.io/adk-docs/tools/function-tools/)
