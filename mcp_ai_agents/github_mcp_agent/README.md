# 🐙 GitHub MCP Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-an-mcp-github-agent-in-less-than-50-lines-of-code)，通过详细的代码讲解、说明和最佳实践，从零开始学习如何构建这个项目。**

这是一个基于 Streamlit 的应用，通过模型上下文协议（MCP），让你能够使用自然语言查询探索和分析 GitHub 仓库。

**✨ 现已使用 GitHub 官方的 [GitHub MCP Server](https://github.com/github/github-mcp-server)！**

## 功能

- **自然语言界面**：使用普通语言询问仓库相关问题
- **全面分析**：探索 Issues、Pull Requests、仓库活动以及代码统计信息
- **交互式 UI**：提供示例查询和自定义输入的友好界面
- **MCP 集成**：利用模型上下文协议与 GitHub API 交互
- **实时结果**：即时获取仓库活动和健康状况洞察

## 配置

### 环境要求

- Python 3.8+
- Docker（用于官方 GitHub MCP Server）
  - 可从 [docker.com](https://www.docker.com/get-started) 下载并安装
  - 启动应用前请确保 Docker 正在运行
- OpenAI API Key
- GitHub Personal Access Token

### 安装

1. 克隆仓库：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd mcp-github-agent
   ```

2. 安装所需 Python 包：
   ```bash
   pip install -r requirements.txt
   ```

3. 验证 Docker 已安装并正在运行：
   ```bash
   docker --version
   docker ps
   ```

4. 获取 API Key：
   - **OpenAI API Key**：从 [platform.openai.com/api-keys](https://platform.openai.com/api-keys) 获取
   - **GitHub Token**：在 [github.com/settings/tokens](https://github.com/settings/tokens) 创建，并授予 `repo` Scope

### 运行应用

1. 启动 Streamlit 应用：
   ```bash
   streamlit run github_agent.py
   ```

2. 在应用界面中：
   - 输入 OpenAI API Key
   - 输入 GitHub Token
   - 指定要分析的仓库
   - 选择查询类型或输入自定义问题
   - 点击“Run Query”

### 示例查询

#### Issues
- “按标签显示 Issues”
- “哪些 Issues 正在被积极讨论？”
- “查找标记为 Bug 的 Issues”

#### Pull Requests
- “哪些 PR 需要 Review？”
- “显示最近已合并的 PR”
- “查找存在冲突的 PR”

#### Repository
- “显示仓库健康指标”
- “显示仓库活动模式”
- “分析代码质量趋势”
