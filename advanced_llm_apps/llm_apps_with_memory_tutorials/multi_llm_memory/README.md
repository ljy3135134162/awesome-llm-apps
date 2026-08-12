## 🧠 带共享记忆的多 LLM 应用

这个 Streamlit 应用演示了一个带共享记忆层的多 LLM 系统，用户可以在不同语言模型之间切换交互，同时跨会话保留对话历史和上下文。

### 功能

- 支持多个 LLM：
  - OpenAI GPT-4o
  - Anthropic Claude 3.5 Sonnet
- 使用 Qdrant 向量数据库实现持久化记忆
- 按用户保存独立的对话历史
- 检索历史记忆，为当前回答补充上下文
- 提供支持 LLM 切换的友好界面

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/llm_apps_with_memory_tutorials/multi_llm_memory
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 确保 Qdrant 正在运行：

应用默认连接运行在 `localhost:6333` 的 Qdrant。如果你的环境不同，请在代码中调整相应配置。

```bash
docker pull qdrant/qdrant
docker run -p 6333:6333 qdrant/qdrant
```

4. 运行 Streamlit 应用

```bash
streamlit run multi_llm_memory.py
```
