# 支持工单 Agent

本示例展示如何使用 OpenAI Agents SDK 和 Pydantic Schema 构建结构化输出 Agent，用于自动生成客户支持工单。

## 🎯 本示例展示的内容

- **结构化输出**：使用 Pydantic 模型定义响应格式
- **枚举类型**：通过受控枚举定义优先级
- **可选字段**：同时处理必填与可选属性
- **字段验证**：使用类型、描述和验证规则约束输出

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
   from agents import Runner
   from agent import root_agent

   result = Runner.run_sync(root_agent, "I can't log into my account and it's urgent!")
   print(result.final_output)  # 返回 SupportTicket 对象
   ```

## 💡 核心概念

- **Pydantic 模型**：定义稳定、类型安全的输出 Schema
- **Enum 验证**：限制优先级只能为 `low`、`medium`、`high`、`critical` 等预定义值
- **字段描述**：帮助模型理解每个字段应填写什么内容
- **可选字段**：区分必须生成的数据和可缺省的数据

## 🧪 示例输出

Agent 会返回类似下面的 `SupportTicket` 对象：

```python
{
    "title": "Account Login Issue",
    "description": "User unable to access account",
    "priority": "high",
    "category": "account_access",
    "steps_to_reproduce": [
        "Go to login page",
        "Enter credentials",
        "Error occurs"
    ],
    "estimated_resolution_time": "2-4 hours"
}
```

这种输出可以直接交给后续代码、数据库、工单系统或其他 Agent 使用，而不需要再从自然语言中二次解析字段。

## 🧠 为什么使用结构化输出

普通文本响应适合直接展示给用户，但在自动化工作流中，后续程序通常需要稳定的数据结构。Pydantic Schema 可以让 Agent 按固定格式输出，从而降低解析错误并提高系统可靠性。

```text
用户描述问题
     │
     ▼
Support Ticket Agent
     │
     ▼
Pydantic Schema 验证
     │
     ▼
结构化 SupportTicket 对象
     │
     ├──► 工单系统
     ├──► 数据库
     └──► 后续 Agent / 工作流
```

## 🔗 后续步骤

- [产品评论 Agent](../2_2_product_review_agent/README.md) —— 更复杂的结构化输出
- [Email Generator Agent](../README.md) —— 基础结构化输出示例
- [教程 3：工具调用 Agent](../../3_tool_using_agent/README.md) —— 为 Agent 添加工具与函数
