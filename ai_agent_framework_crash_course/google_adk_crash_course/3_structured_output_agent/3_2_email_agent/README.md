# 📧 使用结构化输出的邮件生成 Agent

本教程演示如何使用 Google ADK（Agent Development Kit）框架实现结构化输出。示例通过邮件生成 Agent 展示如何结合 Pydantic Schema 与 Gemini 3 Flash 模型，创建类型安全、结构明确的响应。

## 教程功能

- 📝 **结构化输出实现**：
  - 学习如何使用 Pydantic Schema 获得类型安全的输出
  - 理解如何定义结构化响应格式
  - 了解 Google ADK 如何处理结构化响应

- 🎯 **邮件生成示例**：
  - 以邮件生成为实际应用场景
  - 展示如何生成结构规范的专业邮件内容
  - 演示结构化输出在真实业务中的使用方式

- 🔧 **Google ADK 最佳实践**：
  - 使用清晰指令定义简洁的 Agent
  - 正确使用输出 Schema 提高结果可靠性
  - 通过最小代码量展示核心概念

## 🚀 快速开始

1. **配置环境**：
   ```bash
   cd 3_2_email_agent

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
   2. 选择 `email_generator_agent`
   3. 输入邮件请求，例如：`Write a professional email to schedule a meeting with a client`
   4. Agent 会返回包含 `subject` 和 `body` 字段的结构化 JSON

## 教程概览

本教程演示 Google ADK 中结构化输出的实现方式：

1. **Agent 定义**：学习如何使用 Gemini 3 Flash 创建 `LlmAgent`
2. **输出 Schema**：理解如何使用 Pydantic 模型定义结构化响应
3. **指令设计**：了解如何编写适合结构化输出的清晰 Prompt
4. **结构化响应**：学习如何处理符合既定 Schema 的 JSON 输出

## 代码结构

- `agent.py`：包含主要 Agent 定义和 Pydantic Schema
- `__init__.py`：模块初始化文件，便于导入

## 依赖

- `google-adk`：Google Agent Development Kit
- `pydantic`：数据验证与设置管理

## 结构化输出的工作原理

本教程展示 Google ADK 如何处理结构化输出：

1. **输入处理**：接收自然语言请求并交给 Agent 处理
2. **内容生成**：使用 Gemini 3 Flash 根据指令生成内容
3. **输出结构化**：按照 Pydantic Schema 自动组织响应
4. **响应验证**：确保输出符合定义的结构和数据类型

这种方式可以在 Google ADK 应用中构建更加可靠、类型安全的响应。
