# 🧲 AI 竞品情报智能体团队

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-an-ai-competitor-intelligence-agent-team)，通过详细的代码讲解、说明和最佳实践，从零开始构建本项目。**

AI 竞品情报智能体团队是一款由 Firecrawl 和 Agno AI Agent 框架驱动的强大竞品分析工具。该应用可以从竞争对手网站中提取结构化数据，并利用 AI 生成可执行的洞察，帮助企业分析竞争对手。

## 功能特性

- **多智能体系统**
    - **Firecrawl 智能体**：专门负责抓取并总结竞争对手网站
    - **分析智能体**：生成详细的竞争分析报告
    - **对比智能体**：创建竞争对手之间的结构化对比

- **竞争对手发现**：
  - 使用 Exa AI 通过 URL 匹配寻找相似公司
  - 根据业务描述发现竞争对手
  - 自动提取相关竞争对手 URL

- **综合分析**：
  - 提供结构化分析报告，包括：
    - 市场空白与机会
    - 竞争对手弱点
    - 推荐功能
    - 定价策略
    - 增长机会
    - 可执行建议

- **交互式分析**：用户可以输入公司 URL 或公司描述进行分析

## 环境要求

应用需要以下 Python 库：

- `agno`
- `exa-py`
- `streamlit`
- `pandas`
- `firecrawl-py`

同时还需要以下 API 密钥：
- OpenAI
- Firecrawl
- Exa

## 运行方法

按照以下步骤配置并运行应用：

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_competitor_intelligence_agent_team
   ```

2. **安装依赖**：
    ```bash
    pip install -r requirements.txt
    ```

3. **配置 API 密钥**：
    - 获取 OpenAI API 密钥：https://platform.openai.com/api-keys
    - 获取 Firecrawl API 密钥：[Firecrawl 网站](https://www.firecrawl.dev/app/api-keys)
    - 获取 Exa API 密钥：[Exa 网站](https://dashboard.exa.ai/api-keys)

4. **运行 Streamlit 应用**：
    ```bash
    streamlit run competitor_agent_team.py
    ```

## 使用方法

1. 在侧边栏中输入 API 密钥
2. 输入以下任意一种信息：
   - 你公司的网站 URL
   - 你公司的业务描述
3. 点击“Analyze Competitors（分析竞争对手）”生成：
   - 竞争对手对比表
   - 详细分析报告
   - 战略建议
