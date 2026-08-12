# 🌐 MCP 工具集成

欢迎阅读 **Model Context Protocol（MCP）** 集成指南。本示例演示如何通过标准化的 MCP 协议，将 ADK Agent 连接到外部数据源和工具。

## 🎯 你将学到什么

- **MCP 基础**：理解 Model Context Protocol
- **ADK ↔ MCP 集成**：使用 `MCPToolset` 连接 MCP Server
- **访问外部工具**：调用 MCP Server 暴露的工具
- **服务器通信**：同时支持本地和远程 MCP Server
- **真实应用场景**：通过文件系统和 Firecrawl 示例理解实际用法

## 🧠 核心概念：Model Context Protocol

**Model Context Protocol（MCP）** 是一种开放标准，使 AI Agent 能够：
- 以统一方式访问外部数据源
- 调用远程服务器提供的工具
- 与不同应用进行通信
- 在交互过程中保持上下文

### MCP 如何与 ADK 配合

```text
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  ADK Agent  │◄──►│ MCPToolset  │◄──►│ MCP Server  │
│             │    │             │    │             │
│   Gemini    │    │   Bridge    │    │   Tools     │
└─────────────┘    └─────────────┘    └─────────────┘
```

**MCPToolset** 充当桥接层，负责：
- 连接本地或远程 MCP Server
- 自动发现可用工具
- 将 MCP 工具转换成 ADK 可使用的格式
- 管理连接生命周期

## 🔧 集成模式

### 1. **使用外部 MCP Server**
可以连接已有的 MCP Server，例如：
- **Filesystem Server**：文件操作
- **Wikipedia Server**：知识检索
- **Database Server**：数据访问
- **API Server**：外部服务集成

### 2. **通信协议**
- **Server-Sent Events（SSE）**：用于远程服务器的实时通信
- **Standard I/O（stdio）**：用于本地 MCP Server 进程通信

## 🚀 本教程示例

### 📍 **示例 1：Filesystem Agent**
**位置**：`./filesystem_agent/`
- 连接 Filesystem MCP Server
- 执行文件操作（读取、写入、列目录）
- 处理本地文件系统交互
- 使用 Standard I/O 通信

### 🔥 **示例 2：Firecrawl Agent**
**位置**：`./firecrawl_agent/`
- 连接 Firecrawl MCP Server，实现高级网页抓取
- 支持单页抓取、批量处理和网站爬取
- 通过 AI 分析提取结构化数据
- 执行多来源综合的深度 Web 调研
- 使用 Standard I/O 通信，并结合云端 API

## 📁 项目结构

```text
4_4_mcp_tools/
├── README.md                    # 本文件：MCP 集成指南
├── requirements.txt             # MCP 依赖
├── filesystem_agent/            # Filesystem MCP 集成
│   ├── __init__.py              # 包初始化
│   ├── agent.py                 # 主 Agent 实现
│   └── README.md                # Filesystem Agent 指南
├── firecrawl_agent/             # Firecrawl 网页抓取集成
│   ├── __init__.py              # 包初始化
│   ├── agent.py                 # 主 Agent 实现
│   └── README.md                # Firecrawl Agent 指南
```

## 🎯 核心特性

- **无缝集成**：`MCPToolset` 负责处理 MCP 协议细节
- **自动发现**：自动发现工具并提供给 Agent 使用
- **多种协议**：支持 stdio 和 SSE 通信
- **错误处理**：具备针对网络和服务器问题的错误管理能力
- **资源管理**：正确清理连接和相关资源

## 📋 前置条件

运行示例前：

1. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

2. **配置环境**：
   ```bash
   # 在教程根目录执行
   cp env.example .env
   # 编辑 .env 并添加 Google AI API Key
   ```

3. **安装 Node.js（社区 MCP Server 需要）**：
   ```bash
   # 如果尚未安装，请先安装 Node.js
   # npm/npx 类型的 MCP Server 依赖 Node.js
   ```

## 🔄 工作流程

### 连接流程
1. 使用连接参数**初始化 MCPToolset**
2. 与 MCP Server **建立连接**
3. 通过 MCP 协议**发现工具**
4. 将工具**适配为 ADK 格式**
5. 在 Agent 对话中**调用工具**
6. 完成后**清理连接**

### 代码示例
```python
from google.adk.agents import LlmAgent
from google.adk.tools.mcp_tool.mcp_toolset import MCPToolset, StdioServerParameters

# 为外部服务器创建 MCP Toolset
toolset = MCPToolset(
    connection_params=StdioServerParameters(
        command='npx',
        args=['-y', '@modelcontextprotocol/server-filesystem', '/path/to/folder']
    )
)

# 创建使用 MCP 工具的 Agent
agent = LlmAgent(
    model='gemini-3-flash-preview',
    name='mcp_agent',
    instruction='Use MCP tools to help users',
    tools=[toolset]
)
```

## 🚀 快速开始

### 快速体验
1. **选择一个示例**：
   - **Filesystem Agent**：用于文件操作
   - **Firecrawl Agent**：用于高级网页抓取和研究

2. 按照各示例目录中的指南完成配置

3. **通过 ADK Web 运行**：
   ```bash
   # 在教程根目录执行
   adk web
   ```

## 🔗 示例说明

### Filesystem Agent 示例
```python
# 连接 Filesystem MCP Server
toolset = MCPToolset(
    connection_params=StdioServerParameters(
        command='npx',
        args=['-y', '@modelcontextprotocol/server-filesystem', '/path/to/folder']
    )
)

# 让 Agent 调用文件系统工具
# "List files in the current directory"
# "Read the contents of sample.txt"
```

### Firecrawl Agent 示例
```python
# 连接 Firecrawl MCP Server
toolset = MCPToolset(
    connection_params=StdioServerParameters(
        command='npx',
        args=['-y', 'firecrawl-mcp'],
        env={'FIRECRAWL_API_KEY': 'your_api_key'}
    )
)

# 让 Agent 调用网页抓取工具
# "Scrape the homepage of https://example.com"
# "Find all blog post URLs on https://blog.example.com"
# "Search for recent AI research papers and extract summaries"
# "Extract product details from this e-commerce page: [URL]"
```

## 💡 最佳实践

- **连接管理**：正确管理连接生命周期
- **错误处理**：为网络问题实现稳健的异常处理
- **资源清理**：使用正确的资源清理模式
- **安全性**：验证输入，并正确处理认证信息
- **性能**：高吞吐场景可考虑连接池等方案

## 🔍 故障排查

### 常见问题
- **连接错误**：检查 Server URL 和网络连接
- **找不到工具**：确认 Server 正在运行并已正确暴露工具
- **认证失败**：确认 API Key 和凭据配置正确
- **版本兼容性**：检查 MCP 协议版本是否兼容

### 调试命令
```bash
# 测试 MCP Server 连接
npx @modelcontextprotocol/inspector

# 查看 ADK Agent 调试日志
adk web --debug
```

## 🔗 后续教程

完成本教程后，可以继续学习：
- **[教程 5：Memory Agent](../../5_memory_agent/README.md)** —— 添加记忆能力
- **[教程 7：Plugins](../../7_plugins/README.md)** —— 学习插件机制和跨流程能力
- **[教程 8：Simple Multi-Agent](../../8_simple_multi_agent/README.md)** —— 构建多 Agent 协作系统

## 📚 更多资源

- **[MCP Specification](https://modelcontextprotocol.io/docs/spec)** —— MCP 协议规范
- **[ADK MCP Documentation](https://google.github.io/adk-docs/tools/mcp-tools/)** —— ADK MCP 集成文档
- **[Community MCP Servers](https://github.com/modelcontextprotocol/servers)** —— 可直接使用的社区 MCP Server

## 🎯 真实应用场景

MCP 工具可用于：
- **知识检索**：访问 Wikipedia、数据库、文档
- **文件操作**：读取、写入和管理文件及目录
- **API 集成**：连接外部服务和 API
- **数据处理**：转换并分析来自不同来源的数据
- **自定义工具**：创建并在不同 Agent 之间共享专业工具
