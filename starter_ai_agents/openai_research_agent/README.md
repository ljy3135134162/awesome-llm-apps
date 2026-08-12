# OpenAI 研究 Agent

这是一个使用 OpenAI Agents SDK 和 Streamlit 构建的多 Agent 研究应用。它通过多个专业化 AI Agent 协作，帮助用户针对任意主题进行较为完整的研究。

### 功能特性

- 多 Agent 架构：
    - Triage Agent：规划研究方法并协调整体流程
    - Research Agent：搜索 Web 并收集相关信息
    - Editor Agent：将收集到的事实整理为完整报告

- 自动事实收集：在研究过程中记录重要事实，并保留来源归属
- 结构化报告生成：生成包含标题、大纲和来源引用的清晰报告
- 交互式 UI：使用 Streamlit 构建，方便输入研究主题并查看结果
- Tracing 与监控：为整个研究工作流集成追踪能力

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/openai_research_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI](https://platform.openai.com/) 账号并获取 API Key。
- 设置 `OPENAI_API_KEY` 环境变量。

```bash
export OPENAI_API_KEY='your-api-key-here'
```

4. 运行 AI Agent 团队

```bash
streamlit run research_agent.py
```

随后打开浏览器并访问终端中显示的地址，通常为 `http://localhost:8501`。

### 研究流程

- 在侧边栏输入研究主题，或选择已有示例
- 点击 `Start Research` 开始研究
- 在 `Research Process` 标签页中实时查看研究过程
- 完成后切换到 `Report` 标签页，查看并下载生成的报告
