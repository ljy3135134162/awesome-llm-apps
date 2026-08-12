# 🔬 基于 Google Interactions API 的 AI 研究规划与执行 Agent

这是一个基于 **Google Interactions API** 构建的精简型多阶段研究 Agent，用于展示有状态对话、模型混用、后台执行以及自动生成信息图等能力。

## 🌟 功能特性

- **📋 阶段 1 - 研究规划**：使用 **Gemini 3 Flash** 创建结构化、可执行的研究计划
- **🔍 阶段 2 - 任务选择与深度研究**：选择指定任务，并通过内置 Web 搜索的 **Deep Research Agent** 执行研究
- **📊 阶段 3 - 综合分析 + TL;DR**：使用 **Gemini 3 Pro** 生成执行摘要，并通过 **Gemini 3 Pro Image** 自动生成信息图
- **🎨 自动生成信息图**：在每份报告顶部创建白板风格的 TL;DR 摘要图
- **🔄 有状态对话**：通过 `previous_interaction_id` 在不同阶段之间保持上下文
- **⚡ 后台执行**：支持异步研究执行和进度跟踪
- **📥 导出报告**：可将完整研究报告下载为 Markdown 文件

## 🎯 工作流程

```text
用户研究目标
    ↓
[阶段 1] Gemini 3 Flash → 研究计划
    ↓
[阶段 2] 选择任务 → Deep Research Agent → 研究结果
    ↓
[阶段 3] Gemini 3 Pro → 执行摘要
         + Gemini 3 Pro Image → TL;DR 信息图
```

### 阶段 1：规划
1. 输入研究目标
2. **Gemini 3 Flash** 创建一个包含 5-8 个具体任务的编号研究计划
3. 研究计划会作为一个 `Interaction` 保存，以便后续阶段继续沿用上下文

### 阶段 2：选择任务并执行研究
1. 查看研究计划，并通过复选框选择需要执行的任务
2. 可选择或取消任务，以聚焦研究范围
3. **Deep Research Agent** 使用 `previous_interaction_id` 执行完整的 Web 深度研究

### 阶段 3：综合分析与信息图
1. **Gemini 3 Pro** 将研究结果整合为执行级报告
2. **Gemini 3 Pro Image** 自动生成白板风格的 TL;DR 信息图
3. 报告顶部显示信息图，下方显示完整文本
4. 支持下载为 Markdown 文件

## 🛠️ 技术栈

| 组件 | 技术 |
|---|---|
| **规划模型** | `gemini-3-flash-preview` |
| **研究 Agent** | `deep-research-pro-preview-12-2025` |
| **综合分析模型** | `gemini-3-pro-preview` |
| **信息图模型** | `gemini-3-pro-image-preview` |
| **UI 框架** | Streamlit |
| **Python SDK** | `google-genai` |

### 如何开始？

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/research_agent_gemini_interaction_api
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 Google API Key

- 注册 [Google AI Studio](https://ai.google.dev/) 并获取 API Key。

4. 运行 Streamlit 应用：

```bash
streamlit run research_planner_executor_agent.py
```

5. 在浏览器中打开 `http://localhost:8501`

6. 在侧边栏输入 Google API Key，然后开始研究。

## 📝 示例研究目标

- “研究德国 B2B HR SaaS 市场：主要厂商、监管要求和定价模式”
- “分析 AI 客服工具的市场机会”
- “调查电商可持续包装领域的竞争格局”
- “研究面向 Z 世代的 Fintech 产品所需遵守的监管要求”

## ⚠️ 注意事项

- **Beta API**：Interactions API 当前仍处于 Beta 阶段，功能可能发生变化
- **Deep Research**：完整研究可能需要数分钟
- **Agent 与 Model 的区别**：Deep Research 使用 `agent` 参数，而不是 `model`
- **图像生成**：信息图生成使用标准 `generate_content` API

## 🔗 相关资源

- [Gemini Interactions API Docs](https://ai.google.dev/gemini-api/docs/interactions)
- [Gemini Models](https://ai.google.dev/gemini-api/docs/models)
- [Google AI Studio](https://ai.google.dev/)

## 📄 许可证

本项目属于 [Awesome LLM Apps](https://github.com/Shubhamsaboo/awesome-llm-apps) 项目集合的一部分。
