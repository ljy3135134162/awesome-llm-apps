## 📰 多智能体 AI 研究助手

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-a-multi-agent-llm-app-with-gpt-4o)，通过详细的代码讲解、说明和最佳实践，从零开始构建本项目。**

这个 Streamlit 应用通过一组基于 GPT-4o 的 AI 助手，让你可以研究 HackerNews 上的热门故事和用户。

### 功能特性
- 研究 HackerNews 上的热门故事和用户
- 使用专门负责故事研究和用户研究的 AI 助手团队
- 根据你的研究问题生成博客文章、报告和社交媒体内容

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/multi_agent_apps/multi_agent_researcher
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册一个 [OpenAI 账户](https://platform.openai.com/)（也可以使用你选择的其他 LLM 提供商）并获取 API 密钥。

4. 运行 Streamlit 应用
```bash
streamlit run research_agent.py
```

### 工作原理

- 启动应用后，系统会提示你输入 OpenAI API 密钥。该密钥用于身份验证并访问 OpenAI 语言模型。
- 提供有效的 API 密钥后，系统会创建三个专业 AI 智能体：
    - **HackerNews 研究智能体**：使用 HackerNews API 获取 HackerNews 上的热门故事。
    - **网络搜索智能体**：使用 DuckDuckGo 搜索主题相关的补充信息。
    - **文章阅读智能体**：使用 newspaper4k 工具读取文章 URL 并提取内容。

- 这些智能体会在 **HackerNews 团队** 的协调下共同工作，由该团队统一编排整个研究流程。
- 在提供的文本输入框中输入研究问题，可以是主题、关键词，或者与 HackerNews 故事或用户相关的具体问题。
- HackerNews 团队会按照结构化工作流执行：
    1. 首先根据你的查询在 HackerNews 中搜索相关故事
    2. 使用文章阅读智能体从故事 URL 中提取详细内容
    3. 使用网络搜索智能体收集更多背景和补充信息
    4. 最终生成具有标题、摘要和参考链接的完整且有吸引力的总结
- 生成的内容会以 Article 形式组织，包括标题、摘要和参考链接，便于查看和后续使用。
