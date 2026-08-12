# 🎯 教程 2：结构化输出 Agent

学习如何使用 Pydantic 模型创建能够返回**类型安全、结构化数据**的 Agent。本教程将帮助你确保 Agent 始终返回一致、经过验证的 JSON 响应，使应用程序能够稳定处理这些结果。

## 🎯 你将学到什么

- **结构化输出**：使用 Pydantic 模型定义响应 Schema
- **类型安全**：保证数据类型一致并自动执行验证
- **JSON Schema 生成**：根据 Python 类自动生成 Schema
- **输出验证**：使用内置验证和错误处理机制

## 🧠 核心概念：为什么需要结构化输出？

传统 AI 响应通常是非结构化文本，不便于程序直接处理。结构化输出可以解决这一问题：

- **一致性**：始终返回固定的数据结构
- **验证能力**：自动进行类型检查和数据验证
- **易于集成**：更方便连接数据库、API 和业务应用
- **可靠性**：减少解析错误，提高应用稳定性

```text
┌─────────────────────────────────────────────────────────────┐
│                  非结构化输出 vs 结构化输出                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  非结构化输出：                                             │
│  "The customer John Doe submitted a high priority           │
│   billing issue about charges on January 15th..."           │
│                                                             │
│  结构化输出：                                               │
│  {                                                          │
│    "customer_name": "John Doe",                             │
│    "issue_type": "billing",                                 │
│    "priority": "high",                                      │
│    "date_submitted": "2024-01-15",                          │
│    "description": "Incorrect charges on account"            │
│  }                                                          │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 教程概览

本教程包含三个结构化输出示例：

### **1. Support Ticket Agent**（`2_1_support_ticket_agent/`）
- 使用 Enum 的基础结构化输出
- 必填字段和可选字段
- 业务验证模式

### **2. Product Review Agent**（`2_2_product_review_agent/`）
- 较复杂的情感分析 Schema
- 列表字段和嵌套验证
- 评分分类逻辑

### **3. Email Generator Agent**（`2_3_email_generator_agent/`）
- 简单的双字段结构
- 使用 Enum 验证语气
- 内容格式化模式

## 📁 项目结构

```text
2_structured_output_agent/
├── README.md                    # 本文件：概念说明
├── requirements.txt             # 依赖
├── 2_1_support_ticket_agent/    # 基础结构化输出
│   ├── __init__.py
│   └── agent.py                 # Support Ticket Schema
├── 2_2_product_review_agent/    # 复杂结构化输出
│   ├── __init__.py
│   └── agent.py                 # Product Review 分析
├── 2_3_email_generator_agent/   # 简单结构化输出
│   ├── __init__.py
│   └── agent.py                 # Email 内容生成
├── app.py                       # Streamlit Web 界面（可选）
└── env.example                  # 环境变量模板
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何为 Agent 输出定义 Pydantic 模型
- ✅ 如何使用 Agent 的 `output_type` 参数
- ✅ 如何设计包含嵌套模型的复杂数据结构
- ✅ 如何使用 Enum 限制可选值范围
- ✅ 如何处理可选字段和默认值
- ✅ 如何编写自定义验证逻辑

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

4. **测试 Support Ticket Agent**：
   ```bash
   python support_ticket_agent.py
   ```

5. **测试 Product Review Agent**：
   ```bash
   python product_review_agent.py
   ```

6. **运行交互式 Web 界面**：
   ```bash
   streamlit run app.py
   ```

## 🧪 示例应用场景

### Support Ticket Agent
可以尝试以下客户投诉：
- `My billing statement shows duplicate charges for last month's subscription`
- `I can't log into my account and need immediate help`
- `The app keeps crashing when I try to upload files`

### Product Review Agent
可以尝试以下商品评价：
- `This laptop is amazing! Great battery life and super fast. Would definitely recommend. 5 stars!`
- `The phone camera quality is poor and battery drains quickly. Not worth the price.`
- `Decent product but shipping took forever. Customer service was helpful though.`

## 🔧 常见 Pydantic 模式

### 1. **带 Enum 的基础模型**
```python
class Priority(str, Enum):
    LOW = "low"
    MEDIUM = "medium"
    HIGH = "high"
    CRITICAL = "critical"

class SupportTicket(BaseModel):
    priority: Priority
    category: str
```

### 2. **带默认值的可选字段**
```python
class Review(BaseModel):
    rating: int = Field(ge=1, le=5)
    sentiment: str
    recommend: Optional[bool] = None
```

### 3. **复杂嵌套结构**
```python
class ProductReview(BaseModel):
    product_info: ProductInfo
    review_data: ReviewData
    analysis: ReviewAnalysis
```

## 🔗 后续步骤

完成本教程后，可以继续：
- **[教程 3：工具调用 Agent](../3_tool_using_agent/README.md)** —— 添加自定义工具和函数
- **[教程 4：Runner 执行方式](../4_running_agents/README.md)** —— 掌握不同执行模式
- **[教程 5：上下文管理](../5_context_management/README.md)** —— 管理多轮交互中的状态

## 💡 实用建议

- **先设计 Schema**：实现前先规划数据结构
- **使用清晰字段名和描述**：有助于提高 Agent 输出准确性
- **验证约束**：使用 Pydantic Validator 实现业务规则
- **处理可选字段**：为缺失或不确定数据做好设计
- **测试边界情况**：尝试不完整、含糊或异常输入

## 🚨 故障排查

- **Validation Error**：检查 Pydantic 模型是否与预期输出一致
- **缺少字段**：确认 Schema 中所有必填字段都能被生成
- **类型不匹配**：确认字段类型与返回数据一致
- **Enum 错误**：确认 Enum 值完全匹配，并注意大小写
