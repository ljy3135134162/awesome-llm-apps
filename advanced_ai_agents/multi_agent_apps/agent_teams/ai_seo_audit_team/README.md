# 🔍 AI SEO 审计团队

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-seo-audit-team-with-gemini)，通过详细的代码讲解、说明和最佳实践，从零开始构建这个 AI SEO 审计团队。**

**AI SEO 审计团队** 是一个基于 Google ADK 构建的自主多智能体工作流。它接收网页 URL，抓取实时页面内容，研究当前 SERP 竞争情况，并生成结构清晰、带优先级的 SEO 优化报告。应用通过 **MCP（Model Context Protocol）调用 Firecrawl** 进行准确的网页抓取，并使用 Google Gemini 2.5 Flash 完成分析和报告生成。

## 功能特性

- **端到端站内 SEO 评估**
  - 自动抓取任意公开 URL（Firecrawl MCP）
  - 对标题、标题层级、内容深度、内部/外部链接及技术信号进行结构化审计
- **竞争性 SERP 情报分析**
  - 针对推断出的核心关键词执行 Google 搜索研究
  - 分析排名靠前的竞争对手、内容形式、标题模式以及常见问题
- **可执行优化建议**
  - 输出带优先级的优化路线图，并说明原因及预期影响
  - 提供关键词策略、Schema 机会、内部链接建议和衡量方案
  - 生成适合直接交付给相关人员或用于创建工单的整洁 Markdown 报告
- **ADK Dev UI 集成**
  - 可查看每个智能体步骤的 Trace（抓取 → SERP → 报告）
  - 可通过 `.env` 轻松管理环境变量

## 智能体工作流

| 步骤 | 智能体 | 职责 |
| --- | --- | --- |
| 1 | **Page Auditor Agent（页面审计智能体）** | 调用 `firecrawl_scrape`，检查页面结构，总结技术和内容信号，并推断目标关键词。 |
| 2 | **Serp Analyst Agent（SERP 分析智能体）** | 使用 SERP 数据，提取趋势、机会、PAA 问题及差异化方向。 |
| 3 | **Optimization Advisor Agent（优化顾问智能体）** | 将页面审计和 SERP 洞察整合为 Markdown 报告，并给出明确的优先级和后续行动。 |

所有智能体均通过 ADK 的 `SequentialAgent` 顺序执行，并借助共享 Session 在各阶段之间传递状态。

## 环境要求

### 系统要求
- **Python 3.10+**：用于运行 Google ADK
- **Node.js**：用于通过 npx 启动 Firecrawl MCP Server

### Python 依赖

安装 Python 依赖：

```bash
pip install -r requirements.txt
```

### API Keys

你需要有效的 API Key：

- `GOOGLE_API_KEY` – Gemini（Google AI Studio），用于 LLM 和 Google Search
- `FIRECRAWL_API_KEY` – Firecrawl MCP Server（[点击这里获取](https://firecrawl.dev/app/api-keys)）

设置环境变量，例如添加到 Shell 配置文件，或直接在终端中执行：

```bash
export GOOGLE_API_KEY=your_gemini_key
export FIRECRAWL_API_KEY=your_firecrawl_key
```

你也可以将这些变量写入 `.env` 文件。

## 使用 ADK Dev UI 运行应用

1. **进入项目目录**（建议先激活虚拟环境）：
   ```bash
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_seo_audit_team
   ```

2. **安装依赖**（如果尚未安装）：
   ```bash
   pip install -r requirements.txt
   ```

3. **从项目根目录启动 ADK Web UI**：
   ```bash
   adk web
   ```

4. 在 UI 中：
   - 选择 `ai_seo_audit_team` 应用。
   - 根据提示输入目标 URL。
   - 在 **Trace** 面板中查看智能体执行过程（Firecrawl → Google Search → Report）。

## 使用提示

- 确保目标 URL 可以公开访问，不需要登录认证。
- 该工作流针对每次运行审计单个 URL 进行了优化；审计不同页面时建议创建新 Session。
- 最终报告可以直接复制到文档、工单系统中，或分享给相关负责人。

## 文件结构

```text
ai_seo_audit_team/
├── agent.py          # 多智能体工作流定义
├── requirements.txt  # 最小依赖集合
├── __init__.py       # 模块初始化
└── README.md         # 当前文档
```

## 后续扩展方向

- 通过 ADK Eval Sets 添加自动化评估，用于回归测试。
- 将 Markdown 报告接入 Slack、邮件连接器或工单系统。
- 如果不想使用 Google Search API，可替换为其他 SERP 服务商，例如 Serper、Tavily。
- 使用共享 Session State 扩展更多智能体，例如内容 Brief 生成器、Schema 构建器等。
