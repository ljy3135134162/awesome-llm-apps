# 产品评论 Agent

本示例展示如何使用更复杂的 Pydantic Schema 构建结构化输出 Agent，对产品评论进行分析并返回稳定、可验证的数据结构。

## 🎯 本示例展示的内容

- **复杂 Schema**：使用包含多种数据类型和字段的 Pydantic 模型
- **列表字段**：通过字符串数组提取产品优点和缺点
- **布尔逻辑**：根据评论内容输出是否推荐
- **情感分析**：自动判断评论的情感倾向

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

   review_text = "This laptop is amazing! Great performance, long battery life, but a bit heavy."
   result = Runner.run_sync(root_agent, f"Analyze this review: {review_text}")
   print(result.final_output)  # 返回 ProductReview 对象
   ```

## 💡 核心概念

- **评分验证**：将评分约束在 1～5 星范围内
- **情感枚举**：自动分类为 positive、negative 或 neutral
- **列表处理**：从评论中提取多个优点和缺点
- **可选字段**：处理评论者名称等可能缺失的信息

## 🧪 示例输出

```python
{
    "product_name": "Gaming Laptop XYZ",
    "rating": 4,
    "summary": "Great performance but heavy design",
    "sentiment": "positive",
    "pros": ["Great performance", "Long battery life", "Good display"],
    "cons": ["Heavy weight", "Expensive price"],
    "recommend": true,
    "reviewer_name": "TechEnthusiast123"
}
```

这里的关键并不是让模型“尽量按照某种格式回答”，而是通过结构化输出 Schema 明确定义结果必须包含哪些字段、每个字段允许什么类型，以及哪些字段可以为空。

## 🧠 为什么使用结构化评论分析

普通文本输出通常需要额外解析，而结构化输出可以直接交给后续代码处理：

```text
用户评论
   │
   ▼
Product Review Agent
   │
   ├── 识别产品
   ├── 提取优点 / 缺点
   ├── 判断情感
   ├── 生成评分
   └── 判断是否推荐
   │
   ▼
ProductReview 对象
   │
   ├── 数据库存储
   ├── 前端展示
   ├── 统计分析
   └── 自动化工作流
```

这种方式尤其适合评论聚合、客服分析、电商数据处理以及需要稳定 JSON/Pydantic 输出的业务系统。

## 🔗 后续步骤

- [Support Ticket Agent](../2_1_support_ticket_agent/README.md) —— 基础结构化输出
- [教程 3：工具调用 Agent](../../3_tool_using_agent/README.md) —— 为 Agent 添加工具和函数
