# 🚀 OpenAI Agents SDK 速成课程

这是一套从基础到高级概念全面学习 OpenAI Agents SDK 的教程系列。本速成课程旨在帮助你从零开始掌握如何使用 OpenAI Agents SDK 构建 AI Agent。

## 📚 什么是 OpenAI Agents SDK？

OpenAI Agents SDK 是一个用于**开发和部署 AI Agent**的强大框架，提供以下能力：

### 主要特性

- **Agent 编排**：创建并管理智能 AI Agent
- **工具集成**：通过自定义工具和内置工具扩展 Agent 能力
- **结构化输出**：使用 Pydantic 模型获得类型安全的响应
- **多 Agent 工作流**：通过 Handoff 协调多个 Agent
- **实时执行**：支持同步、异步和流式执行方式
- **语音集成**：支持静态、流式以及实时语音能力
- **Session 管理**：自动维护对话记忆和历史记录
- **生产级能力**：内置 Tracing、Guardrail 和监控

## 🎯 学习路线

本速成课程通过动手实践教程覆盖 OpenAI Agents SDK 的核心概念：

### 📚 **教程列表**

#### **🌱 基础层**

1. **[1_starter_agent](./1_starter_agent/README.md)** —— 你的第一个 OpenAI Agent
   - 基础 Agent 创建与配置
   - 理解不同的执行方式
   - 简单文本处理与响应

2. **[2_structured_output_agent](./2_structured_output_agent/README.md)** —— 类型安全的响应
   - **Support Ticket Agent** —— 将投诉转换为结构化工单
   - **Product Review Agent** —— 从评论中提取结构化数据
   - Pydantic 模型与数据验证

#### **🔧 核心能力层**

3. **[3_tool_using_agent](./3_tool_using_agent/README.md)** —— Agent 工具与函数
   - 使用 `@function_tool` 创建自定义函数工具
   - 内置工具（WebSearch、CodeInterpreter、FileSearch）
   - 工具集成与执行模式

4. **[4_running_agents](./4_running_agents/README.md)** —— 掌握 Agent 运行与执行
   - Agent Loop：LLM 调用、工具执行和 Handoff
   - 同步、异步和流式执行方式
   - 高级流式事件与异常处理
   - Run 配置与对话管理

5. **[5_context_management](./5_context_management/README.md)** —— 状态与上下文管理
   - 在多次运行之间传递上下文
   - 状态持久化与管理
   - 对话流程控制

#### **🧠 高级功能层**

6. **[6_guardrails_validation](./6_guardrails_validation/README.md)** —— 安全与验证
   - 用于验证用户输入的 Input Guardrail
   - 用于过滤响应的 Output Guardrail
   - 自定义业务规则验证

7. **[7_sessions](./7_sessions/README.md)** —— Session 与记忆管理
   - 使用 SQLiteSession 自动保存对话历史
   - 记忆操作与对话纠正
   - 多 Session 管理与组织

#### **🤝 多 Agent 层**

8. **[8_handoffs_delegation](./8_handoffs_delegation/README.md)** —— Agent Handoff 与任务委派
   - Agent 之间的任务委派
   - 分诊系统与智能路由
   - 带回调的高级 Handoff 配置

9. **[9_multi_agent_orchestration](./9_multi_agent_orchestration/README.md)** —— 复杂工作流
   - 使用 `asyncio.gather()` 并行执行多个 Agent
   - 将 Agent 作为工具的编排模式
   - 多阶段工作流协调

#### **🔍 生产层**

10. **[10_tracing_observability](./10_tracing_observability/README.md)** —— 监控与调试
    - 内置 Tracing 与执行过程可视化
    - 为复杂工作流创建自定义 Trace 和 Span
    - 性能监控与优化

#### **🎙️ 语音与高级功能**

11. **[11_voice](./11_voice/README.md)** —— 语音 Agent 与实时对话
    - 静态语音处理（轮次式交互）
    - 流式语音处理（实时对话）
    - 实时语音 Agent（通过 WebSocket 实现超低延迟）
    - 语音转文本、文本转语音以及语音流水线

## 🛠️ 前置条件

开始本速成课程前，请确保你已经具备：

- 已安装 **Python 3.8+**（语音功能需要 Python 3.9+）
- 从 [OpenAI Platform](https://platform.openai.com/api-keys) 获取的 **OpenAI API Key**
- 对 Python 和 API 有基础了解
- 熟悉 async/await 概念会更有帮助，但不是必须条件
- **语音教程额外需要**：麦克风以及扬声器/耳机

## 📖 如何使用本课程

每个教程都采用一致的结构：

- **README.md**：概念说明与学习目标
- **Python 文件**：包含 Agent 实现和示例
- **交互式界面**：用于动手测试的 Streamlit Web 应用
- **子模块**：针对不同概念组织的示例
- **requirements.txt**：教程依赖
- **env.example**：环境变量模板

### 学习方式

1. **阅读 README**，理解相关概念
2. **查看代码**，了解具体实现
3. **运行示例**，观察 Agent 的实际行为
4. **修改代码进行实验**
5. **使用交互式界面**进行动手测试
6. 使用麦克风尝试**语音功能**（教程 11）
7. 准备好后**进入下一个教程**

## 🎯 教程特色

每个教程都包含：

- ✅ **清晰的概念说明**
- ✅ **最小化、可运行的代码示例**
- ✅ **真实使用场景**
- ✅ **分步骤说明**
- ✅ **交互式 Web 界面**
- ✅ **最佳实践与技巧**

## 🚀 快速开始

1. **克隆仓库**并进入当前目录
2. 从上面的列表中**选择一个教程**
3. 按照对应教程的 **README** 操作
4. **安装依赖**：`pip install -r requirements.txt`
5. **配置环境**：将 `env.example` 复制为 `.env`，并填入你的 API Key
6. **运行示例**并开始学习

## 🔧 环境配置

每个教程都需要 OpenAI API Key。请在各教程目录中创建 `.env` 文件：

```bash
OPENAI_API_KEY=sk-your_openai_key_here
```

可以从这里获取 API Key：[https://platform.openai.com/api-keys](https://platform.openai.com/api-keys)

## 💡 学习建议

- **按顺序学习**：按照教程顺序进行，学习体验最佳
- **大胆实验**：修改代码并观察结果
- **使用 Web 界面**：交互式应用可以让学习更直观
- **认真阅读错误信息**：其中通常会包含有用的解决提示
- **参与社区**：与其他学习者交流并分享经验

## 🚨 常见问题

### API Key 问题

- 确保 `.env` 文件位于对应教程目录
- 确认 API Key 有效并具备足够额度
- 检查环境变量名称是否存在拼写错误

### Import 错误

- 确保已经安装依赖：`pip install -r requirements.txt`
- 确认正在使用 Python 3.8 或更高版本
- 如果依赖冲突，可以尝试创建虚拟环境

### Rate Limit

- OpenAI 会根据你的套餐设置速率限制
- 如果触发限制，可以稍后再试
- 如需更高限制，可以考虑升级 OpenAI 套餐

## 📚 更多资源

- [OpenAI Agents SDK 文档](https://openai.github.io/openai-agents-python/)
- [OpenAI Platform](https://platform.openai.com/)
- [Pydantic 文档](https://docs.pydantic.dev/)
- [Streamlit 文档](https://docs.streamlit.io/)

## 🤝 贡献

欢迎提交改进、Bug 修复或新增教程。每个教程应满足以下要求：

- 可独立运行
- 包含清晰文档
- 遵循现有结构
- 使用尽可能精简且易于理解的代码

## 📊 学习进度

可以使用下面的清单跟踪课程进度：

- [ ] **教程 1**：基础 Agent 创建 ✨
- [ ] **教程 2**：使用 Pydantic 实现结构化输出
- [ ] **教程 3**：工具集成与自定义函数
- [ ] **教程 4**：掌握执行方式
- [ ] **教程 5**：上下文与状态管理
- [ ] **教程 6**：Guardrail 与验证
- [ ] **教程 7**：Session 与记忆管理
- [ ] **教程 8**：Agent Handoff 与任务委派
- [ ] **教程 9**：多 Agent 编排
- [ ] **教程 10**：Tracing 与可观测性
- [ ] **教程 11**：语音 Agent 与实时对话 🎯

学习愉快！🚀
