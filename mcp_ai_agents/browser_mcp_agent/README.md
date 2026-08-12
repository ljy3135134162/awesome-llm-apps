# 🌐 Browser MCP Agent

https://github.com/user-attachments/assets/a01e09fa-131b-479a-8df3-2d1a61fd80f3

一个 Streamlit 应用，通过模型上下文协议（MCP）、[MCP-Agent](https://github.com/lastmile-ai/mcp-agent) 和 Playwright 集成，让你可以使用自然语言命令浏览网站并与网页交互。

## 功能

- **自然语言界面**：使用简单的自然语言命令控制浏览器
- **完整浏览器导航**：访问网站并在页面之间导航
- **交互元素操作**：点击按钮、填写表单并滚动浏览内容
- **可视化反馈**：截取网页元素截图
- **信息提取**：提取并总结网页内容
- **多步骤任务**：通过对话完成复杂的浏览操作流程

## 配置

### 环境要求

- Python 3.8+
- Node.js 和 npm（用于 Playwright）
  - 这是关键依赖！应用使用 Playwright 控制无头浏览器
  - 可从 [nodejs.org](https://nodejs.org/) 下载并安装
- OpenAI 或 Anthropic API Key

### 安装

1. 克隆仓库：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd mcp_ai_agents/browser_mcp_agent
   ```

2. 安装所需 Python 包：
   ```bash
   pip install -r requirements.txt
   ```

3. 验证 Node.js 和 npm 已安装：
   ```bash
   node --version
   npm --version
   ```
   两条命令都应返回版本号。如果没有，请安装 Node.js。

4. 配置 API Key。选择以下一种方式：

   **a) 使用环境变量（OpenAI 最简单的方式）：**
   ```bash
   export OPENAI_API_KEY=your-openai-api-key
   ```

   **b) 使用 `mcp_agent.secrets.yaml`（Ollama / 任意自定义 base URL 必需）：**
   ```bash
   cp mcp_agent.secrets.yaml.example mcp_agent.secrets.yaml
   # 编辑 mcp_agent.secrets.yaml，将密钥填写到 openai.api_key 下
   ```

### 使用本地 Ollama 模型运行

由于 `mcp-agent` 使用 OpenAI 兼容端点，而 Ollama 在 `http://localhost:11434/v1` 提供兼容接口，因此只需修改配置即可让该 Agent 使用本地模型运行——无需修改代码或安装额外依赖。相关讨论见 [#329](https://github.com/Shubhamsaboo/awesome-llm-apps/issues/329)。

1. 安装并启动 Ollama，然后拉取支持工具调用的模型：
   ```bash
   ollama pull llama3.2
   ollama serve
   ```

2. 编辑 `mcp_agent.config.yaml`，将 `openai:` 配置块替换为：
   ```yaml
   openai:
     base_url: "http://localhost:11434/v1"
     default_model: "llama3.2"
   ```

3. 在 `mcp_agent.secrets.yaml` 中设置任意非空 `api_key`（Ollama 会忽略该值）：
   ```yaml
   openai:
     api_key: "ollama"
   ```

4. 按正常方式运行：`streamlit run main.py`。这种方式不需要设置 `OPENAI_API_KEY` 环境变量。

> 注意：浏览器自动化更适合使用具备较强推理能力的模型。较小的本地模型在执行多步骤 Playwright 任务时可能表现不佳。

### 运行应用

1. 启动 Streamlit 应用：
   ```bash
   streamlit run main.py
   ```

2. 在应用界面中：
   - 输入浏览命令
   - 点击“Run Command”
   - 查看执行结果和截图

### 示例命令

#### 基础导航
- “访问 www.mcp-agent.com”
- “返回上一页”

#### 页面交互
- “点击登录按钮”
- “向下滚动查看更多内容”

#### 内容提取
- “总结这个页面的主要内容”
- “提取导航菜单项”
- “截取首屏区域的截图”

#### 多步骤任务
- “进入博客，找到最新文章，并总结其关键内容”

## 架构

该应用使用：
- Streamlit 构建用户界面
- MCP（模型上下文协议）连接 LLM 与工具
- Playwright 实现浏览器自动化
- [MCP-Agent](https://github.com/lastmile-ai/mcp-agent/) 作为 Agent 框架
- OpenAI 模型解释命令并生成响应
