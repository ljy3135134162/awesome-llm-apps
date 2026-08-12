## 🛒 带记忆能力的 AI 客户支持 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-customer-support-agent-with-memory)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 Streamlit 的 AI 客户支持应用，使用 GPT-4o 生成的合成数据进行演示。Agent 采用 OpenAI GPT-4o 模型，并通过 Mem0 保存历史交互记忆，同时使用 Qdrant 作为向量数据库。

### 功能特性

- 提供用于与 AI 客服 Agent 交互的聊天界面
- 持久化保存客户历史交互与用户画像
- 支持生成合成数据，用于测试与演示
- 使用 OpenAI GPT-4o 生成智能回答

### 如何开始？

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/ai_customer_support_agent
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 确保 Qdrant 正在运行：

默认情况下，应用会连接运行在 `localhost:6333` 的 Qdrant。如果你的部署方式不同，请相应修改代码中的配置。

```bash
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \
    -v "$(pwd)/qdrant_storage:/qdrant/storage:z" \
    qdrant/qdrant
```

4. 运行 Streamlit 应用：

```bash
streamlit run customer_support_agent.py
```
