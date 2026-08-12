## 📈 AI 创业趋势分析 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-startup-trend-analysis-agent-using-claude-3-5-sonnet)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

AI Startup Trend Analysis Agent 是一个面向创业者的趋势分析工具，可识别新兴趋势、潜在市场空白和特定行业中的增长机会，并生成可执行的洞察。创业者可以利用这些数据驱动的结果验证创业想法、发现市场机会，并为创业项目做出更有依据的决策。

该 Agent 结合 Newspaper4k 与 DuckDuckGo，扫描并分析与创业相关的文章和市场数据，再使用 Claude 3.5 Sonnet 对这些信息进行处理，提取新兴模式，帮助创业者识别更有潜力的创业方向。

### 功能特性
- **用户提示词**：创业者可以输入希望研究的特定创业领域或技术方向。
- **新闻采集**：使用 DuckDuckGo 收集近期创业新闻、融资轮次和市场分析。
- **摘要生成**：使用 Newspaper4k 对已验证的信息生成简洁摘要。
- **趋势分析**：从分析内容中识别创业融资、技术采用和市场机会方面的新兴趋势。
- **Streamlit UI**：提供基于 Streamlit 构建的易用交互界面。

### 如何开始

1. **克隆仓库**：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_startup_trend_analysis_agent
```

2. **创建并激活虚拟环境**：

```bash
# macOS/Linux
python -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
.\venv\Scripts\activate
```

3. **安装所需依赖**：

```bash
pip install -r requirements.txt
```

4. **运行应用**：

```bash
streamlit run startup_trends_agent.py
```

### 重要说明
- 本项目使用 Claude API 进行高级语言处理。可从 [Anthropic 官网](https://www.anthropic.com/api) 获取 Anthropic API Key。
