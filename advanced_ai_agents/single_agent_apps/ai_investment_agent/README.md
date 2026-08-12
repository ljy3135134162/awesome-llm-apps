## 📈 AI 投资分析 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-ai-investment-agent-with-gpt-4o)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 Agno AgentOS 框架构建的 AI 投资分析 Agent，可分析股票并生成详细的投资报告。应用结合 GPT-5.2 与 Yahoo Finance 数据，为投资决策提供结构化参考信息。

### 功能特性
- 对比两只股票的表现
- 获取完整的公司信息
- 获取最新公司新闻与分析师建议
- 使用 AgentOS 提供 Web 交互界面

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_investment_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI](https://platform.openai.com/) 账号并获取 API Key。
- 导出 API Key：

```bash
export OPENAI_API_KEY="your-api-key-here"
```

4. 运行 AgentOS 应用

```bash
python investment_agent.py
```

5. 打开浏览器并访问终端输出的 URL，即可通过 Playground 界面与 AI 投资 Agent 交互。

6. 连接 AgentOS

如果希望通过浏览器中的 AgentOS Control Plane 管理、监控和使用该金融 Agent，需要连接当前运行中的 AgentOS 实例。

**分步指南：**

- 查看官方文档：[Connecting Your OS](https://docs.agno.com/agent-os/connecting-your-os)
- 按照文档完成本地 AgentOS 注册并建立连接。
