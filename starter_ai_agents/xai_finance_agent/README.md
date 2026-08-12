## 📊 基于 xAI Grok 的 AI 金融 Agent

这个应用构建了一个由 xAI Grok 模型驱动的金融分析 Agent，并结合实时股票数据与 Web 搜索能力，通过交互式 Playground 提供结构化金融分析结果。

### 功能特性

- 使用 xAI 的 Grok-4 Fast 模型
- 通过 YFinance 分析实时股票数据
- 通过 DuckDuckGo 执行 Web 搜索
- 使用表格格式展示金融数据
- 提供交互式 Playground 界面

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/xai_finance_agent
```

2. 安装所需依赖：

```bash
cd awesome-llm-apps/starter_ai_agents/xai_finance_agent
pip install -r requirements.txt
```

3. 获取 xAI API Key

- 注册 [xAI API](https://console.x.ai/) 账号并获取 API Key。
- 设置 `XAI_API_KEY` 环境变量。

```bash
export XAI_API_KEY='your-api-key-here'
```

4. 运行 AI Agent

```bash
python xai_finance_agent.py
```

5. 打开浏览器，并访问控制台输出中提供的 URL，即可通过 Playground 界面与 AI 金融 Agent 交互。

6. 连接 AgentOS

如果希望通过浏览器中的 AgentOS Control Plane 管理、监控并与金融 Agent 交互，需要将正在运行的 AgentOS 实例连接到控制平面。

**分步说明：**

- 查看官方文档：[Connecting Your OS](https://docs.agno.com/agent-os/connecting-your-os)
- 按照文档中的步骤注册本地 AgentOS 并建立连接。
