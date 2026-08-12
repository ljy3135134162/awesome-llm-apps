## 💻 使用 RAG 的本地 Llama-3.1

这是一个 Streamlit 应用，可使用本地运行的 Llama-3.1 和检索增强生成（RAG）与任意网页内容进行对话。该应用完全运行在你的计算机上，100% 免费，并且无需互联网连接即可进行模型推理。

### 功能
- 输入网页 URL
- 针对网页内容提出问题
- 使用 RAG 和本地运行的 Llama-3.1 模型获得准确回答

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/llama3.1_local_rag
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 运行 Streamlit 应用：

```bash
streamlit run llama3.1_local_rag.py
```

### 工作原理

- 应用使用 WebBaseLoader 加载网页数据，并通过 RecursiveCharacterTextSplitter 将内容切分为多个文本块。
- 使用 Ollama 创建 Embedding，并通过 Chroma 构建向量存储。
- 应用建立 RAG（检索增强生成）链，根据用户问题检索相关文档。
- 调用 Llama-3.1 模型，根据检索得到的上下文生成答案。
- 最终在应用界面中显示用户问题的回答。
