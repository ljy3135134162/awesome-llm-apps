# 🚀 Google ADK 速成课程

这是一套从基础到高级概念全面学习 Google Agent Development Kit（ADK）的教程系列。本速成课程旨在帮助你从零开始掌握如何使用 Google ADK 构建 AI Agent。

> **📌 注意：本课程已经更新为使用新的 Gemini 3 Flash 模型！**  
> 本课程中的所有教程都使用 **Gemini 3 Flash** 模型（例如 `gemini-3-flash-preview`）。

## 📚 什么是 Google ADK？

Google ADK（Agent Development Kit）是一个灵活、模块化的框架，用于**开发和部署 AI Agent**。它针对 Gemini 和 Google 生态进行了优化，但同时具备**模型无关**和**部署无关**特性，因此也能兼容其他框架。

### 主要特性

- **灵活编排**：可以通过工作流 Agent 或由 LLM 驱动的动态路由定义流程
- **多 Agent 架构**：使用多个专业化 Agent 构建模块化应用
- **丰富的工具生态**：使用内置工具、自定义函数，或集成第三方库
- **可直接部署**：支持容器化，并可将 Agent 部署到任意环境
- **内置评估**：系统化评估 Agent 性能
- **安全与保障**：内置用于构建可信 Agent 的安全模式

## 🎯 学习路线

本速成课程通过动手实践教程覆盖 Google ADK 的核心概念：

### 📚 **教程列表**

1. **[1_starter_agent](./1_starter_agent/README.md)** —— 你的第一个 ADK Agent
   - 创建基础 Agent
   - 理解 ADK 工作流程
   - 简单文本处理

2. **[2_model_agnostic_agent](./2_model_agnostic_agent/README.md)** —— 模型无关的 Agent 开发
   - **[2.1 OpenAI Agent](./2_model_agnostic_agent/README.md)** —— OpenAI 集成
   - **[2.2 Anthropic Claude Agent](./2_model_agnostic_agent/README.md)** —— Claude 集成

3. **[3_structured_output_agent](./3_structured_output_agent/README.md)** —— 类型安全的响应
   - **[3.1 客户支持工单 Agent](./3_structured_output_agent/3_1_customer_support_ticket_agent/README.md)** —— Pydantic Schema
   - **[3.2 Email Agent](./3_structured_output_agent/3_2_email_agent/README.md)** —— 结构化数据验证

4. **[4_tool_using_agent](./4_tool_using_agent/README.md)** —— 使用工具的 Agent
   - **[4.1 内置工具](./4_tool_using_agent/4_1_builtin_tools/README.md)** —— 搜索、代码执行
   - **[4.2 函数工具](./4_tool_using_agent/4_2_function_tools/README.md)** —— 自定义 Python 函数
   - **[4.3 第三方工具](./4_tool_using_agent/4_3_thirdparty_tools/README.md)** —— LangChain、CrewAI
   - **[4.4 MCP 工具](./4_tool_using_agent/4_4_mcp_tools/README.md)** —— MCP 工具集成

5. **[5_memory_agent](./5_memory_agent/README.md)** —— 记忆与会话管理
   - **[5.1 内存会话](./5_memory_agent/5_1_in_memory_conversation_agent/README.md)** —— 基础 Session 管理
   - **[5.2 持久化会话](./5_memory_agent/5_2_persistent_conversation_agent/README.md)** —— 使用 SQLite 进行数据库存储

6. **[6_callbacks](./6_callbacks/README.md)** —— 回调模式与监控
   - **[6.1 Agent 生命周期回调](./6_callbacks/6_1_agent_lifecycle_callbacks/README.md)** —— 监控 Agent 创建与清理
   - **[6.2 LLM 交互回调](./6_callbacks/6_2_llm_interaction_callbacks/README.md)** —— 跟踪模型请求与响应
   - **[6.3 工具执行回调](./6_callbacks/6_3_tool_execution_callbacks/README.md)** —— 监控工具调用与结果

7. **[7_plugins](./7_plugins/README.md)** —— 用于横切关注点的插件系统
   - 全局回调管理
   - 请求/响应修改
   - 错误处理与日志记录
   - 使用情况分析与监控

8. **[8_simple_multi_agent](./8_simple_multi_agent/README.md)** —— 多 Agent 编排
   - **[8.1 多 Agent 研究器](./8_simple_multi_agent/README.md)** —— 由专业化 Agent 组成的研究流水线
   - 使用子 Agent 的协调器 Agent
   - 顺序工作流：研究 → 总结 → 批判
   - Web 搜索集成与全面分析

9. **9_multi_agent_patterns** —— 多 Agent 模式
   - **[9.1 Sequential Agent](./9_multi_agent_patterns/9_1_sequential_agent/README.md)** —— 确定性的子 Agent 流水线（例如：起草 → 批判 → 改进）
   - **[9.2 Loop Agent](./9_multi_agent_patterns/9_2_loop_agent/README.md)** —— 带明确停止条件的迭代优化流程（达到最大迭代次数或调用退出工具）。教程使用推文创作循环演示该模式。
   - **[9.3 Parallel Agent](./9_multi_agent_patterns/9_3_parallel_agent/README.md)** —— 并发执行多个子 Agent 并合并结果。

## 🛠️ 前置条件

开始本速成课程前，请确保你已经具备：

- 已安装 **Python 3.11+**
- 从 [Google AI Studio](https://aistudio.google.com/) 获取的 **Google AI API Key**
- 对 Python 和 API 有基础了解

## 📖 如何使用本课程

每个教程都采用一致的结构：

- **README.md**：概念说明与学习目标
- **Python 文件**：包含 Agent 实现和 Streamlit 应用
- **requirements.txt**：教程所需依赖

### 学习方式

1. **阅读 README**，理解相关概念
2. **查看代码**，了解具体实现
3. **运行示例**，观察实际效果
4. **修改代码进行实验**
5. 准备好后**进入下一个教程**

## 🎯 教程特色

每个教程都包含：

- ✅ **清晰的概念说明**
- ✅ **最小化、可运行的代码示例**
- ✅ **真实使用场景**
- ✅ **分步骤说明**
- ✅ **最佳实践与技巧**

## 📚 更多资源

- [Google ADK 文档](https://google.github.io/adk-docs/)
- [Google AI Studio](https://aistudio.google.com/)
- [Gemini API 参考文档](https://ai.google.dev/docs)
- [Pydantic 文档](https://docs.pydantic.dev/)

## 🤝 贡献

欢迎提交改进、Bug 修复或新增教程。每个教程应满足以下要求：

- 可独立运行
- 包含清晰文档
- 遵循现有结构
- 使用尽可能精简且易于理解的代码
