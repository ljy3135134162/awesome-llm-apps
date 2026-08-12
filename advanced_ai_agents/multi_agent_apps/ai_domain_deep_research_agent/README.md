# 🔍 AI 领域深度研究 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-domain-deep-research-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个高级 AI 研究 Agent，基于 Agno Agent 框架、Together AI 的 Qwen 模型以及 Composio 工具构建。它可以围绕任意主题自动生成研究问题，通过多个搜索渠道寻找答案，并将研究结果整理为专业报告，同时支持集成 Google Docs。

## 功能特性

- 🧠 **智能研究问题生成**
  - 根据你的研究主题自动生成 5 个具体问题
  - 根据指定领域调整问题方向
  - 重点生成便于得出明确结论的 Yes/No 类型问题

- 🔎 **多来源研究**
  - 使用 Tavily Search 获取广泛 Web 搜索结果
  - 使用 Perplexity AI 进行更深入分析
  - 综合多个来源提升研究完整度

- 📊 **专业报告生成**
  - 将研究结果整理成类似 McKinsey 风格的报告
  - 包含执行摘要、详细分析和结论等结构
  - 自动创建包含完整报告的 Google Doc

- 🖥️ **易用界面**
  - 提供清晰的 Streamlit 操作界面
  - 实时显示研究进度
  - 可展开查看每个阶段的详细结果

## 运行方式

1. **配置环境**

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/multi_agent_apps/ai_domain_deep_research_agent

# 安装依赖
pip install -r requirements.txt

composio add googledocs
composio add perplexityai
```

2. **配置 API Keys**

- 从 [Together AI](https://together.ai) 获取 Together AI API Key
- 从 [Composio](https://composio.ai) 获取 Composio API Key
- 将这些 Key 写入 `.env` 文件，或者直接在应用侧边栏中输入

3. **运行应用**

```bash
streamlit run ai_domain_deep_research_agent.py
```

## 使用方式

1. 使用上述命令启动应用。
2. 在侧边栏输入 Together AI 和 Composio API Key。
3. 在主界面输入研究主题和所属领域。
4. 点击 `Generate Research Questions` 自动生成研究问题。
5. 检查生成的问题，然后点击 `Start Research` 开始研究。
6. 研究完成后，点击 `Compile Final Report` 生成最终专业报告。
7. 可直接在应用中查看报告，也可以通过 Google Docs 打开完整文档。

## 技术细节

- **Agno Framework**：用于创建和编排 AI Agent
- **Together AI**：提供 Qwen 3 235B 模型，用于高级语言处理
- **Composio Tools**：集成搜索引擎及 Google Docs 功能
- **Streamlit**：提供交互式 Web 界面

## 示例使用场景

- **学术研究**：快速收集不同学科主题的研究资料
- **市场分析**：调查市场趋势、竞争对手和行业发展
- **政策研究**：分析政策影响及相关历史背景
- **技术评估**：研究新兴技术及其潜在影响

## 依赖

- `agno`
- `composio_agno`
- `streamlit`
- `python-dotenv`

## 许可证

本项目属于 `awesome-llm-apps` 项目集合，并采用 MIT License。
