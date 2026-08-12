# 🎫 使用结构化输出的客户支持工单 Agent

本教程演示如何使用 Google ADK（Agent Development Kit）框架实现结构化的客户支持工单系统。示例展示如何结合 Pydantic Schema 与 Gemini 3 Flash 模型，创建具备优先级、分类和预计解决时间等字段的类型安全结构化工单。

## 教程功能

- 🎫 **结构化支持工单**：
  - 学习如何创建完整的支持工单 Schema
  - 理解优先级和分类方式
  - 了解如何估算问题解决时间

- 🔧 **高级 Schema 设计**：
  - 使用枚举和可选字段构建复杂 Pydantic 模型
  - 正确配置字段验证与说明
  - 获得类型安全的结构化响应

- 🎯 **真实业务应用**：
  - 实用的客户支持场景
  - 展示如何处理不同类型的支持请求
  - 演示结构化输出如何用于业务流程

- 📊 **优先级管理**：
  - 四级优先级体系（低、中、高、严重）
  - 根据问题描述自动分配优先级
  - 根据问题分类路由到不同部门

## 🚀 快速开始

1. **配置环境**：
   ```bash
   cd 3_1_customer_support_ticket_agent

   # 复制环境变量模板
   cp env.example .env

   # 编辑 .env 并添加 Google AI API Key
   # API Key 获取地址：https://aistudio.google.com/
   ```

2. **安装依赖**：
   ```bash
   # 返回上一级目录
   cd ..

   # 安装所需包
   pip install -r requirements.txt
   ```

3. **运行 Agent**：
   ```bash
   # 启动 ADK Web 界面
   adk web
   ```
   然后：
   1. 在浏览器中打开 Web 界面
   2. 选择 `support_ticket_creator` Agent
   3. 输入支持请求，例如：`I can't log into my account and I have an important meeting in 2 hours`
   4. Agent 将返回包含完整工单信息的结构化 JSON

## 教程概览

本教程演示 Google ADK 中较高级的结构化输出实现：

1. **复杂 Schema 设计**：学习如何创建功能完善的 Pydantic 模型
2. **枚举使用**：理解如何使用 Enum 约束字段值
3. **可选字段**：了解如何通过合理的默认值处理可选数据
4. **业务逻辑**：学习如何实现真实业务流程

## 代码结构

- `customer_support_agent/agent.py`：包含主要 Agent 定义和 `SupportTicket` Schema
- `customer_support_agent/__init__.py`：模块初始化文件，便于导入

## 支持工单 Schema

Agent 创建的结构化工单包含以下字段：

- **title**：问题的简短摘要
- **description**：详细问题描述
- **priority**：优先级（low、medium、high、critical）
- **category**：所属部门（Technical、Billing、Account、Product）
- **steps_to_reproduce**：技术问题的可选复现步骤列表
- **estimated_resolution_time**：预计解决时间

## 使用示例

**输入**：`My payment failed and I'm getting charged twice for the same service`

**输出**：
```json
{
  "title": "Duplicate payment charge issue",
  "description": "Customer reports payment failure followed by duplicate charges for the same service",
  "priority": "high",
  "category": "Billing",
  "steps_to_reproduce": null,
  "estimated_resolution_time": "4-6 hours"
}
```

## 依赖

- `google-adk`：Google Agent Development Kit
- `pydantic`：数据验证与设置管理

## 结构化输出的工作原理

本教程展示 Google ADK 如何处理复杂结构化输出：

1. **输入处理**：接收自然语言形式的支持请求
2. **上下文分析**：分析问题严重程度和问题类型
3. **结构化生成**：生成包含全部必要字段的完整工单
4. **验证**：确保输出符合定义的 Schema 和业务规则

这种方式展示了如何在 Google ADK 应用中构建可靠、可直接用于业务系统的结构化响应。
