# 基于 YAML 的多 Agent Web 研究系统

这是一个使用 Google ADK 构建的多 Agent 系统，通过 Firecrawl MCP 工具进行网页抓取，并协调专门的研究 Agent 与摘要 Agent 完成完整的研究流程。

## 系统架构

该系统由以下组件组成：

1. **主协调 Agent**（`root_agent.yaml`）：负责整体工作流编排
2. **研究 Agent**（`research_agent.yaml`）：使用 Firecrawl MCP 工具进行网页抓取与内容分析
3. **摘要 Agent**（`summary_agent.yaml`）：生成完整报告与摘要
4. **Firecrawl MCP 集成**：为子 Agent 提供高级网页抓取能力

## 功能特性

- 🔍 **高级网页抓取**：使用 Firecrawl MCP 工具可靠提取网页内容
- 🔬 **智能内容分析**：研究 Agent 提取洞察、模式和关键数据
- 📝 **完整报告生成**：摘要 Agent 输出结构化报告和建议
- 🤖 **多 Agent 协同**：主 Agent 负责协调完整工作流
- 🔐 **安全的 API 管理**：通过环境变量管理 Firecrawl API Key
- ⚡ **子 Agent MCP 支持**：在子 Agent 中正确配置 MCP 工具

## 配置

### 前置条件

1. 安装 Google ADK：
   ```bash
   pip install google-adk
   ```

2. 从 [firecrawl.dev](https://firecrawl.dev) 获取 Firecrawl API Key。

3. 在 `.env` 文件中配置环境变量：

   **方案 A：Google AI Studio（推荐用于开发）**
   ```bash
   GOOGLE_GENAI_USE_VERTEXAI=0
   GOOGLE_API_KEY=<your-google-gemini-api-key>
   FIRECRAWL_API_KEY=<your-firecrawl-api-key>
   ```

   **方案 B：Vertex AI（推荐用于生产）**
   ```bash
   GOOGLE_GENAI_USE_VERTEXAI=1
   GOOGLE_CLOUD_PROJECT=<your-gcp-project-id>
   GOOGLE_CLOUD_LOCATION=us-central1
   FIRECRAWL_API_KEY=<your-firecrawl-api-key>
   ```

   **API Key 获取方式：**
   - **Google AI Studio**：从 [Google AI Studio](https://aistudio.google.com/app/apikey) 获取 API Key
   - **Vertex AI**：参考 [Google Cloud Authentication](https://cloud.google.com/vertex-ai/generative-ai/docs/start/api-keys) 配置认证
   - **Firecrawl**：从 [Firecrawl](https://firecrawl.dev/app/api-keys) 获取 API Key

### 安装与检查

1. 进入 Agent 目录：
   ```bash
   cd ai_agent_framework_crash_course/google_adk_crash_course/adk_yaml_examples/multi_agent_web_research_team/multi_agent_web_researcher
   ```

2. 验证 ADK 是否安装成功：
   ```bash
   adk --version
   ```

## 使用方式

### 运行 Agent

可通过以下任一种方式运行：

1. **Web 界面**（推荐用于测试）：
   ```bash
   adk web
   ```

2. **命令行**：
   ```bash
   adk run
   ```

3. **API Server**（用于系统集成）：
   ```bash
   adk api_server
   ```

## Agent 配置说明

### 主 Agent（`root_agent.yaml`）

协调 Agent 负责：
- 将任务委派给专用子 Agent
- 协调研究 Agent 与摘要 Agent
- 汇总并生成最终综合报告
- 向子 Agent 提供明确指令

### 研究 Agent（`research_agent.yaml`）

专门负责网页抓取和内容分析：
- **Firecrawl MCP 工具**：使用 `firecrawl_scrape` 和 `firecrawl_search`
- **内容分析**：提取关键发现和洞察
- **模式识别**：识别趋势和关联关系
- **数据提取**：突出重要引用和数据点
- **后续研究建议**：提出进一步调查方向

**可用 Firecrawl 工具：**
- `firecrawl_scrape`：抓取单个 URL 的内容
- `firecrawl_search`：搜索相关网页内容
- `firecrawl_batch_scrape`：批量抓取多个 URL
- `firecrawl_map`：发现网站中的 URL
- `firecrawl_crawl`：执行完整网站爬取

### 摘要 Agent（`summary_agent.yaml`）

专门负责报告生成：
- 创建执行摘要
- 按主题组织信息
- 生成关键结论
- 提供可执行建议

## 工作流

1. **输入**：用户提供 URL 或研究主题
2. **委派**：主 Agent 将明确任务传递给 `research_agent`
3. **网页抓取**：研究 Agent 使用 Firecrawl MCP 工具提取内容
4. **分析**：研究 Agent 对抓取内容进行分析并提炼洞察
5. **摘要**：摘要 Agent 生成完整报告
6. **综合**：主 Agent 将各阶段结果整合为最终报告

## 环境变量

### Google AI Studio 配置
| 变量 | 说明 | 必需 |
|---|---|---|
| `GOOGLE_GENAI_USE_VERTEXAI` | 使用 Google AI Studio 时设置为 0 | 是 |
| `GOOGLE_API_KEY` | AI Studio 提供的 Google Gemini API Key | 是 |
| `FIRECRAWL_API_KEY` | 用于网页抓取的 Firecrawl API Key | 是 |

### Vertex AI 配置
| 变量 | 说明 | 必需 |
|---|---|---|
| `GOOGLE_GENAI_USE_VERTEXAI` | 使用 Vertex AI 时设置为 1 | 是 |
| `GOOGLE_CLOUD_PROJECT` | Google Cloud Project ID | 是 |
| `GOOGLE_CLOUD_LOCATION` | GCP 区域，例如 `us-central1` | 是 |
| `FIRECRAWL_API_KEY` | 用于网页抓取的 Firecrawl API Key | 是 |

### 认证方式

**Google AI Studio：**
- 使用简单的 API Key 认证
- 适合开发和测试
- 不需要额外配置 Google Cloud

**Vertex AI：**
- 企业级认证方式
- 更适合生产部署
- 需要配置 Google Cloud Project
- 支持 Grounding、安全设置等高级能力

## 使用示例

### Web 界面
1. 运行 `adk web`
2. 打开终端输出的本地 URL
3. 输入 URL 或研究主题，例如 `Scrape and analyze https://example.com` 或 `Research AI trends`
4. 观察多 Agent 系统完成整个处理流程

### 命令行
```bash
adk run
# 根据提示输入研究查询
```

## 故障排查

### 常见问题

1. **API Key 错误**：确认 `.env` 中已配置全部必需的 API Key
2. **找不到 ADK**：确认已安装 ADK，并激活正确的 Python 环境
3. **Firecrawl 错误**：确认 Firecrawl API Key 有效且账户额度充足
4. **MCP 连接问题**：检查 Node.js 和 npm 是否已正确安装
5. **认证问题**：
   - **Google AI Studio**：确认 API Key 有效并具备正确权限
   - **Vertex AI**：确认 Google Cloud 认证已正确配置，例如执行 `gcloud auth application-default login`
   - **Project ID**：确认 Vertex AI 使用的 Google Cloud Project ID 正确

## 参考资料

- [Google ADK 文档](https://google.github.io/adk-docs/)
- [Agent 配置参考](https://google.github.io/adk-docs/agents/config/#build-an-agent)
- [Firecrawl 文档](https://docs.firecrawl.dev/)
- [MCP 工具](https://modelcontextprotocol.io/)
