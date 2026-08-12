# Multi-MCP Agent Router

一个用于演示 **多 Agent + MCP** 模式的 Streamlit 应用：不同的专业 AI Agent 分别连接不同的 MCP 服务器，用于处理各自领域的任务。

它不是让单个 Agent 持有全部工具，而是由 Router 将你的请求发送给对应的**专业 Agent**——例如代码审查员、安全审计员、研究员或 BIM 工程师；每个 Agent 只拥有完成自身任务所需的 MCP 工具。

## 功能

- **4 个专业 Agent**：代码审查员、安全审计员、研究员和 BIM 工程师
- **MCP 工具路由**：不同 Agent 分别连接不同 MCP 服务器（GitHub、filesystem、fetch 等）
- **Agent 选择**：可根据查询类型自动路由，也可以手动选择 Agent
- **流式响应**：通过 Anthropic API 实时输出 Claude 的响应
- **对话记忆**：在单个会话内为每个 Agent 分别保留对话历史

## 架构

```
用户请求
    |
    v
[Router] --> 判断意图
    |
    +-- 代码审查  --> GitHub MCP + Filesystem MCP
    +-- 安全审计  --> GitHub MCP + Fetch MCP
    +-- 研究      --> Fetch MCP + Filesystem MCP
    +-- BIM/Revit --> 自定义 MCP（命名管道）
```

## 配置

### 环境要求

- Python 3.10+
- Anthropic API Key
- MCP 服务器（可选——即使没有 MCP 服务器，应用也可以运行）

### 安装

1. 克隆仓库：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd mcp_ai_agents/multi_mcp_agent_forge
   ```

2. 安装依赖：
   ```bash
   pip install -r requirements.txt
   ```

3. 运行应用：
   ```bash
   streamlit run agent_forge.py
   ```

4. 在侧边栏输入 Anthropic API Key，然后即可开始提问。

## 工作原理

1. **Agent 定义**：每个 Agent 都有名称、系统提示词和 MCP 服务器配置列表
2. **Router**：判断用户请求类型，并选择最合适的 Agent
3. **MCP 连接**：被选中的 Agent 连接到分配给它的 MCP 服务器
4. **执行**：Claude 在可访问该 Agent 专属工具的情况下处理请求
5. **响应**：结果以流式方式返回 Streamlit UI

## 扩展

可以通过在 `AGENTS` 字典中定义新的 Agent 来扩展：

```python
AGENTS["my_agent"] = Agent(
    name="My Agent",
    description="Handles X tasks",
    system_prompt="You are an expert in X...",
    mcp_servers=[{"command": "npx", "args": ["-y", "@some/mcp-server"]}]
)
```

## 致谢

灵感来自 [cadre-ai/Agent Forge](https://github.com/WeberG619/cadre-ai)——一个面向 Claude Code 的生产级多 Agent 框架，包含 17 个专业 Agent、持久化记忆和桌面自动化能力。
