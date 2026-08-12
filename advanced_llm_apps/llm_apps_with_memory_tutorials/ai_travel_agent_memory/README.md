## 🧳 带记忆的 AI 旅行 Agent

这个 Streamlit 应用实现了一个由 AI 驱动的旅行助手，可以记住用户偏好以及过去的交互。它使用 OpenAI 的 GPT-4o 生成回答，并通过 Mem0 与 Qdrant 保存和管理对话历史。

### 功能

- 基于聊天的 AI 旅行助手交互界面
- 持久保存用户偏好和历史对话
- 使用 OpenAI GPT-4o 模型生成智能回答
- 使用 Mem0 和 Qdrant 实现记忆存储与检索
- 支持按用户查看对话历史和记忆内容

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/llm_apps_with_memory_tutorials/ai_travel_agent_memory
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 确保 Qdrant 正在运行：

应用默认要求 Qdrant 运行在 `localhost:6333`。如果你的环境不同，请修改代码中的相关配置。

```bash
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \
    -v $(pwd)/qdrant_storage:/qdrant/storage:z \
    qdrant/qdrant
```

4. 运行 Streamlit 应用

```bash
streamlit run travel_agent_memory.py
```
