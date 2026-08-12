# 🔥 AI Startup Insight with Firecrawl FIRE-1 Agent

这是一个基于 Firecrawl 的 FIRE-1 Agent、Extract v1 接口和 Agno Agent 框架构建的高级网页提取与分析工具，可快速获取一家新创公司的关键信息。应用能够自动从创业公司网站中提取结构化数据，并通过 AI 完成业务分析，省去大量手动调研工作。

## 功能特性

- 🌐 **智能网页提取**：
  - 从任意公司网站提取结构化数据
  - 自动识别公司信息、使命和产品功能
  - 支持按顺序处理多个网站

- 🔍 **高级网页导航**：
  - 与按钮、链接及动态元素交互
  - 处理分页和多步骤流程
  - 跨多个页面访问信息

- 🧠 **AI 业务分析**：
  - 对提取出的公司数据生成有价值的摘要
  - 识别独特价值主张和市场机会
  - 提供可执行的商业情报

- 📊 **结构化数据输出**：
  - 使用统一 JSON Schema 组织信息
  - 提取公司名称、简介、使命和产品功能
  - 标准化输出，便于后续处理

- 🎯 **交互式 UI**：
  - 易用的 Streamlit 界面
  - 可并行处理多个 URL
  - 清晰展示提取数据和 AI 分析结果

## 如何运行

1. **配置环境**

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_startup_insight_fire1_agent
```

安装依赖：

```bash
pip install -r requirements.txt
```

2. **配置 API Keys**

- 从 [Firecrawl](https://firecrawl.dev) 获取 Firecrawl API Key
- 从 [OpenAI Platform](https://platform.openai.com) 获取 OpenAI API Key

3. **运行应用**

```bash
streamlit run ai_startup_insight_fire1_agent.py
```

## 使用方式

1. 使用上述命令启动应用
2. 在侧边栏中输入 Firecrawl 和 OpenAI API Key
3. 在文本区域中输入一个或多个公司网站 URL，每行一个
4. 点击 `🚀 Start Analysis` 开始提取与分析
5. 在标签页界面中查看每个网站的结构化数据和 AI 分析结果

## 可用于测试的网站示例

- https://www.spurtest.com
- https://cluely.com
- https://www.harvey.ai

## 使用的技术

- **Firecrawl FIRE-1**：高级网页提取 Agent
- **Agno Agent Framework**：提供 AI 分析能力
- **OpenAI GPT Models**：生成商业洞察
- **Streamlit**：交互式 Web 界面

## 环境要求

- Python 3.8+
- Firecrawl API Key
- OpenAI API Key
- 用于网页提取的互联网连接
