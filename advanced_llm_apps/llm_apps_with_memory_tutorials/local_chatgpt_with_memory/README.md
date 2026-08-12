## 🧠 使用 Llama 3.1 和个人记忆的本地 ChatGPT

这个 Streamlit 应用使用 Llama 3.1 实现了一个完全本地运行、类似 ChatGPT 的体验，并为每个用户提供独立的个性化记忆存储。包括语言模型、Embedding 和向量数据库在内的全部组件都在本地运行，不需要任何外部 API Key。

### 功能

- 完全本地实现，不依赖外部 API
- 通过 Ollama 运行 Llama 3.1
- 为每个用户提供独立的个人记忆空间
- 使用 Nomic Embed 在本地生成 Embedding
- 使用 Qdrant 进行向量存储

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_llm_apps/llm_apps_with_memory_tutorials/local_chatgpt_with_memory
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 在本地安装并启动 [Qdrant](https://qdrant.tech/documentation/guides/installation/) 向量数据库

```bash
docker pull qdrant/qdrant
docker run -p 6333:6333 qdrant/qdrant
```

4. 安装 [Ollama](https://ollama.com/download) 并拉取 Llama 3.1

```bash
ollama pull llama3.1
```

> 如果你使用 AMD Strix Halo / Ryzen AI MAX+ 395，可以参考 [AMD Strix Halo 本地 LLM 指南](https://github.com/hogeheer499-commits/strix-halo-guide)。其中记录了经过测试的 Ubuntu 与 Ollama Vulkan/RADV 配置、模型和量化方案、基准测试结果，以及该硬件平台上已知不可行的方案。

5. 运行 Streamlit 应用

```bash
streamlit run local_chatgpt_memory.py
```
