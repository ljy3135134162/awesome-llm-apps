# 使用 Pydantic AI 构建类型化 Agentic RAG

这个 Streamlit 应用可以基于上传的 PDF 或文档 URL 回答问题。
每个响应都会被验证为一个 `Answer` 对象，其中包含精确的来源引用、Chunk ID、置信度分数以及 `answered` 判定。如果检索结果过弱，应用会在调用语言模型之前直接拒绝回答。

![Typed Agentic RAG screenshot placeholder](assets/screenshot-placeholder.svg)

## 功能

- 使用 Pydantic AI 的 `Agent`、`RunContext` 和依赖注入
- 提供带来源元数据和余弦相似度分数的类型化 `retrieve` 工具
- 使用 Pydantic 模型定义答案、引用和检索证据
- 在模型输出验证后，对照已索引 Chunk 检查精确引用文本
- 对知识库之外的问题使用确定性的拒答门控
- 支持 OpenAI 或 Anthropic 作为答案模型
- 使用 OpenAI Embedding；仅配置 Anthropic 时可回退到本地哈希嵌入
- 使用会话级 NumPy 向量存储，无需数据库服务

## 工作原理

1. `rag.py` 从 PDF 或网页中提取文本，将其切分为相互重叠的 Chunk，生成嵌入，并把归一化向量存储在内存中。
2. `agent.py` 通过 `RagDependencies` 注入向量存储。Pydantic AI Agent 必须先调用类型化 `retrieve` 工具，然后才能生成 `Answer`。
3. 预检索会将最高余弦相似度分数与拒答阈值比较。分数过低时直接返回 `answered=False`，不会发送 LLM 请求。
4. 对于正常回答，每条引用都必须匹配已存储的来源、Chunk ID 和逐字引用片段。引用无效或缺失时会转为拒答。
5. `app.py` 负责显示答案、置信度、引用或拒答状态。

当存在 `OPENAI_API_KEY` 时，Auto 模式会使用 Pydantic AI 的 OpenAI `Embedder` 和 `text-embedding-3-small`。如果只有 `ANTHROPIC_API_KEY`，由于 Anthropic 没有 Embedding API，Auto 模式会使用本地哈希后端。本地后端更适合基于关键词的演示；如果需要跨同义改写进行语义检索，请选择 OpenAI Embedding。

## 环境要求

- Python 3.12 或更高版本
- OpenAI API Key 或 Anthropic API Key

## 配置

从仓库根目录执行：

```bash
cd rag_tutorials/agentic_typed_rag_pydanticai
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

在 `.env` 中添加一个 API Key：

```text
OPENAI_API_KEY=your-key
```

或者：

```text
ANTHROPIC_API_KEY=your-key
```

默认答案模型为 `openai:gpt-5.2` 和 `anthropic:claude-sonnet-4-6`。可以在侧边栏修改模型字段，或者将 `RAG_MODEL` 设置为其他 Pydantic AI 模型字符串。

## 运行

在 `rag_tutorials/agentic_typed_rag_pydanticai` 目录中执行：

```bash
streamlit run app.py
```

上传一个或多个 PDF，也可以额外添加文档 URL，然后选择 **Build knowledge base**。提出知识库范围内的问题，可以看到带引用的答案；再提出无关主题的问题，则可以看到拒答状态。

## 测试

确定性测试套件使用 Pydantic AI 的 `TestModel`，因此不会向模型提供商发送请求：

```bash
python3 test_typed_rag.py
```

## 文件结构

```text
agentic_typed_rag_pydanticai/
├── app.py
├── agent.py
├── rag.py
├── test_typed_rag.py
├── requirements.txt
├── .env.example
└── assets/screenshot-placeholder.svg
```

作为 `awesome-llm-apps` 的一部分，本项目采用 Apache-2.0 许可证。
