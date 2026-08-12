## 🗞️ AI 记者 Agent

这是一个基于 Streamlit 的 AI 记者应用，使用 OpenAI GPT-4o 自动完成文章研究、写作和编辑流程，帮助用户快速围绕任意主题生成结构完整、信息充分的高质量文章。

### 功能特性
- 根据给定主题搜索 Web 上的相关信息
- 生成结构清晰、信息充分且具有可读性的文章
- 对生成内容进行编辑和润色，使其达到更高的新闻写作质量标准

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_journalist_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI](https://platform.openai.com/) 账号（或选择其他 LLM Provider）并获取 API Key。

4. 获取 SerpAPI Key

- 注册 [SerpAPI](https://serpapi.com/) 账号并获取 API Key。

5. 运行 Streamlit 应用

```bash
streamlit run journalist_agent.py
```

### 工作原理

AI Journalist Agent 主要由三个组件组成：

- **Searcher**：根据用户给定的主题生成搜索关键词，并使用 SerpAPI 搜索相关网页 URL。
- **Writer**：通过 NewspaperToolkit 获取这些 URL 中的正文内容，并基于提取到的信息撰写文章。
- **Editor**：负责协调 Searcher 与 Writer 的工作流，并对最终生成的文章进行编辑和润色。
