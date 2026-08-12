# 🚀 Multi-MCP 智能助手

Multi-MCP 智能助手是一款强大的生产力工具，通过集成多个 Model Context Protocol（MCP）服务器，让用户可以使用自然语言无缝访问 GitHub、Perplexity、Calendar 和 Gmail 等服务。该高级 AI 助手基于 Agno 的 AI Agent 框架构建，旨在成为数字工作空间中的生产力倍增器。

## 功能

- **多 Agent 系统**
    - **GitHub 集成**：完整的仓库管理、Issue 跟踪和代码分析
    - **Perplexity 研究**：实时网页搜索与信息收集
    - **日历管理**：事件安排与会议协调
    - **Gmail 集成**：邮件管理与沟通工作流

- **核心能力**：
  - 仓库管理（创建、克隆、Fork、搜索）
  - Issue 与 PR 工作流（创建、更新、审查、合并、评论）
  - 实时网页搜索与研究
  - 日程安排与空闲时间管理
  - 邮件整理与自动回复
  - 跨平台工作流自动化

- **高级功能**：
  - 支持流式响应的交互式 CLI
  - 对话记忆与上下文保留
  - 复杂工作流的工具链调用
  - 会话级用户 ID 和 Session ID
  - Markdown 格式响应
  - 主动提供工作流建议

- **生产力导向**：
  - 跨平台自动化（GitHub Issue → Calendar Event）
  - 研究驱动的开发工作流
  - 项目管理集成
  - 文档与知识共享

## 如何运行

按照以下步骤配置并运行应用：

1. **克隆仓库**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd awesome-llm-apps/mcp_ai_agents/multi_mcp_agent
   ```

2. **安装依赖**：
    ```bash
    pip install -r requirements.txt
    ```

3. **验证 Node.js 安装**（MCP 服务器需要）：
    ```bash
    node --version
    npm --version
    npx --version
    ```
    如果尚未安装 Node.js，请前往 [nodejs.org](https://nodejs.org/) 下载。

4. **配置 API Key**：
    在项目目录创建 `.env` 文件，并添加以下变量：
    ```env
    OPENAI_API_KEY=your-openai-api-key
    GITHUB_PERSONAL_ACCESS_TOKEN=your-github-token
    PERPLEXITY_API_KEY=your-perplexity-api-key
    ```

    - OpenAI API Key：https://platform.openai.com/api-keys
    - GitHub Personal Access Token：https://github.com/settings/tokens（需要 `repo`、`user` 和 `admin:org` 权限）
    - Perplexity API Key：https://www.perplexity.ai/
    - 根据你的配置要求设置 OpenAI MCP Headers

5. **运行 Multi-MCP Agent**：
    ```bash
    python multi_mcp_agent.py
    ```

6. **开始交互**：
    - 助手会验证环境变量
    - 生成唯一的用户 ID 和 Session ID
    - 初始化与所有 MCP 服务器的连接
    - 启动交互式 CLI

## 使用方式

1. **环境验证**：助手会自动检查所有必需的 API Key 和环境变量
2. **会话管理**：每个会话都会获得唯一的用户 ID 和 Session ID，用于跟踪和维护上下文
3. **交互命令**：可以使用自然语言操作已集成的服务

### 示例命令

**GitHub 操作**：
- “显示我最近的 GitHub 仓库”
- “在我的项目仓库中创建一个新 Issue”
- “搜索我的仓库中的 Python 代码”
- “查看最新的 Pull Request”

**研究与信息获取**：
- “搜索最新的 AI 发展动态”
- “机器学习领域现在有哪些热门话题？”
- “查找 FastAPI 文档”
- “研究微服务最佳实践”

**日历管理**：
- “安排下周的一场会议”
- “显示我即将到来的日程”
- “查找可以安排 2 小时会议的空闲时间段”

**跨平台工作流**：
- “创建一个 GitHub Issue，并安排后续会议”
- “研究一个主题并生成总结文档”
- “查找热门仓库并添加到我的关注列表”

4. **会话控制**：输入 `exit`、`quit` 或 `bye` 结束会话

## 架构

Multi-MCP Agent 使用：
- **Agno Framework**：负责 Agent 编排和工具管理
- **OpenAI GPT-4o**：作为核心语言模型
- **MCP Servers**：用于外部服务集成
- **异步架构**：提高并发操作效率
- **记忆系统**：保留上下文与对话历史

## 注意事项

该助手通过 Node.js 包连接多个 MCP 服务器。请确保网络连接稳定，并为所有服务配置有效的 API Key。工具链能力支持跨多个平台执行复杂工作流，因此非常适合作为开发者和专业用户的生产力增强工具。
