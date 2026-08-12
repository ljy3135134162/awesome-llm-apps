# 🎯 教程 3：结构化输出 Agent

欢迎进入结构化输出教程。本教程将介绍如何创建返回**类型安全、结构化数据**的 Agent，而不是普通文本。这对于需要稳定、可预测数据格式的应用非常重要。

## 🎯 你将学到什么

- **Pydantic Schema**：定义带验证的数据结构
- **类型安全**：确保 Agent 返回预期的数据格式
- **业务逻辑**：可靠地处理结构化数据
- **错误处理**：优雅处理验证失败
- **真实应用场景**：客户支持与邮件生成

## 🧠 核心概念：结构化输出

结构化输出意味着 Agent 返回的是**经过验证的数据对象**，而不是原始文本：
- ✅ **类型安全**：明确知道会收到什么数据格式
- ✅ **验证机制**：自动检查必填字段和数据类型
- ✅ **可靠性**：无需再手动解析文本响应
- ✅ **易于集成**：可以直接用于应用和数据库

### 为什么使用结构化输出？
- **可预测**：始终获得相同的数据结构
- **经过验证**：Pydantic 自动保证数据正确性
- **类型明确**：获得完整 IDE 支持与类型检查
- **易于扩展**：Schema 可以方便地修改和演进

## 🚀 教程结构

本教程包含**两个完整示例**：

### 📍 **示例 1：客户支持工单 Agent**
**位置**：`./3_1_customer_support_ticket_agent/`
- 从客户投诉中提取结构化工单信息
- 优先级分类和紧急程度判断
- 联系方式提取
- 部门路由逻辑

### 📍 **示例 2：邮件生成 Agent**
**位置**：`./3_2_email_agent/`
- 生成带元数据的结构化邮件内容
- 优化邮件主题
- 收件人分类
- 邮件模板格式化

## 📁 项目结构

```text
3_structured_output_agent/
├── README.md                           # 本教程概览
├── 3_1_customer_support_ticket_agent/  # 客户支持示例
└── 3_2_email_agent/                    # 邮件生成示例
```

每个示例目录都遵循统一结构：
- **Python 文件**：包含 Agent 实现和 Streamlit 应用
- **README.md**：配置与使用说明
- **requirements.txt**：依赖列表

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何为结构化输出定义 Pydantic Schema
- ✅ 如何配置 Agent 返回结构化数据
- ✅ 如何优雅处理验证错误
- ✅ 什么时候应该使用结构化输出，什么时候使用普通文本
- ✅ Schema 设计的最佳实践

## 💡 关键模式

### 基础结构化输出模式
```python
from pydantic import BaseModel
from google.adk.agents import Agent

class TicketInfo(BaseModel):
    title: str
    priority: str
    category: str
    urgency_level: int

agent = Agent(
    name="support_agent",
    model="gemini-3-flash-preview",
    instruction="Extract ticket information...",
    response_format=TicketInfo,  # 确保返回结构化输出
)
```

### 带验证的高级 Schema
```python
from pydantic import BaseModel, Field, validator
from typing import List, Optional

class EmailData(BaseModel):
    subject: str = Field(..., min_length=5, max_length=100)
    recipients: List[str] = Field(..., min_items=1)
    priority: str = Field(..., regex="^(low|medium|high)$")
    
    @validator('recipients')
    def validate_emails(cls, v):
        # 自定义邮箱验证逻辑
        return v
```

## 🎯 真实应用场景

结构化输出 Agent 非常适合：
- **客户支持**：从投诉中提取工单信息
- **数据处理**：将非结构化文本转换为数据库记录
- **内容生成**：生成带元数据的结构化内容
- **表单处理**：从文档中提取信息
- **API 集成**：向其他系统提供稳定一致的数据格式

## 💡 实用建议

- **Schema 要清晰**：使用有意义的字段名，并添加说明
- **合理验证**：根据业务场景添加适当的 Validator
- **可选字段**：可能缺失的字段使用 `Optional`
- **提供示例**：在 Schema 文档中给出示例数据
- **处理错误**：始终为验证失败提供清晰的错误处理逻辑

## 🚨 重要说明

- **需要 Pydantic**：Schema 定义依赖 Pydantic
- **模型支持差异**：不同模型对结构化输出的支持程度并不相同
- **验证开销**：复杂 Schema 可能会增加响应延迟
- **Schema 演进**：生产环境中应提前考虑 Schema 版本变化与兼容性
