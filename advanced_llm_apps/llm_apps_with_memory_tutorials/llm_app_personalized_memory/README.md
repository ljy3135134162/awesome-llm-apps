## 🧠 带记忆功能的 LLM 应用

这是一个基于 Streamlit 的 AI 聊天机器人，使用 OpenAI 的 GPT-4o 模型，并加入持久化记忆能力。用户可以与 AI 持续对话，同时在多次交互之间保留上下文。

### 功能

- 使用 OpenAI GPT-4o 模型生成回答
- 使用 Mem0 和 Qdrant 向量数据库实现持久化记忆
- 支持用户查看自己的历史对话
- 使用 Streamlit 提供易用的交互界面

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/llm_apps_with_memory_tutorials/llm_app_personalized_memory
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 确保 Qdrant 正在运行

应用默认要求 Qdrant 运行在 `localhost:6333`。如果你的环境不同，请修改代码中的相关配置。

```bash
docker pull qdrant/qdrant

docker run -p 6333:6333 -p 6334:6334 \
    -v $(pwd)/qdrant_storage:/qdrant/storage:z \
    qdrant/qdrant
```

4. 运行 Streamlit 应用

```bash
streamlit run llm_app_memory.py
```
