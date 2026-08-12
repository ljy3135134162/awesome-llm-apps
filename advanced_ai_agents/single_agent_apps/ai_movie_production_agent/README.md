## 🎬 AI 电影制作 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-movie-production-agent-with-claude-3-5-sonnet)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 Streamlit 的 AI 电影制作助手，使用 Claude 3.5 Sonnet 帮助你把电影创意进一步发展成完整概念。它可以自动完成剧本框架设计和选角建议，让电影创意的前期策划更高效。

### 功能特性
- 根据电影创意、类型和目标受众生成剧本大纲
- 根据演员过往作品和当前可用性，为主要角色推荐合适演员
- 提供简洁的电影概念总览

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_movie_production_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 Anthropic API Key

- 注册 [Anthropic](https://console.anthropic.com) 账号（或选择其他 LLM Provider）并获取 API Key。

4. 获取 SerpAPI Key

- 注册 [SerpAPI](https://serpapi.com/) 账号并获取 API Key。

5. 运行 Streamlit 应用

```bash
streamlit run movie_production_agent.py
```

### 工作原理

AI Movie Production Agent 由三个主要组件组成：

- **ScriptWriter**：根据电影创意和类型生成剧本大纲，包括角色描述和关键剧情节点。
- **CastingDirector**：结合演员过往表现和当前可用性，为主要角色推荐合适演员。
- **MovieProducer**：负责协调 ScriptWriter 与 CastingDirector 的工作，并生成简洁的电影概念总览。
