# 多模态 Agentic RAG

这是一个使用 Gemini Embedding 2 和 Google ADK 构建的多模态 RAG 应用。你可以添加文本、URL、PDF、图片、音频或视频，提出问题，并获得带有清晰引用依据的回答。

UI 还提供了一个用于检查搜索空间的 3D Embedding 视图。每个数据源显示为一个点；提出问题时，查询也会被投影到同一空间，并高亮显示被引用的数据源。

![架构图](assets/multimodal-agentic-rag-architecture.png)

## 功能

- 在本地内存索引中添加和删除多模态数据源。
- 使用 Gemini Embedding 2 为数据源和查询生成 Embedding。
- 必须配置 `GOOGLE_API_KEY`；应用不使用本地向量或回答回退方案。
- 基于已存储的 Embedding，通过余弦相似度检索证据。
- 运行 Google ADK Agent，根据检索到的上下文协调答案生成。
- 将引用信息与答案正文分开显示，避免引用 ID 干扰回答内容。
- 使用 PCA 将数据源向量和查询向量投影到 3D 空间中进行检查。

## 架构

| 层 | 作用 |
| --- | --- |
| React + Vite 前端 | 数据源管理、问答面板、引用、Trace 和 3D Embedding 视图 |
| FastAPI 后端 | 数据摄取、检索、回答 API 和 Embedding 空间快照 |
| `MultimodalRagStore` | 内存中的数据源元数据、文本块、Embedding、搜索和 PCA 投影 |
| Gemini Embedding 2 | 为支持的多种模态生成数据源和查询 Embedding |
| Google ADK Agent | 回答协调器，接收与 UI 中显示内容相同的检索数据包 |

一个重要的实现细节是：`/ask` 只执行一次检索，然后将同一个检索数据包传递给 ADK 回答流程。因此，最终答案与引用面板都基于同一组经过排序的证据。

## 项目结构

```text
rag_tutorials/multimodal_agentic_rag/
|-- README.md
|-- assets/
|   `-- multimodal-agentic-rag-architecture.png
|-- backend/
|   |-- app_state.py
|   |-- rag_store.py
|   |-- requirements.txt
|   |-- server.py
|   `-- agentic_rag_agent/
|       |-- __init__.py
|       `-- agent.py
`-- frontend/
    |-- index.html
    |-- package.json
    |-- src/
    |   |-- App.tsx
    |   |-- main.tsx
    |   `-- styles.css
    |-- tsconfig.json
    `-- vite.config.ts
```

## 本地运行

启动后端：

```bash
cd rag_tutorials/multimodal_agentic_rag/backend
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export GOOGLE_API_KEY="your-google-ai-studio-key"
python server.py
```

后端运行地址：

```text
http://localhost:8897
```

在另一个终端中启动前端：

```bash
cd rag_tutorials/multimodal_agentic_rag/frontend
npm install
npm run dev -- --port 5177
```

前端运行地址：

```text
http://localhost:5177
```

如果后端使用其他端口：

```bash
VITE_API_URL=http://localhost:8897 npm run dev -- --port 5177
```

## 试用

1. 打开 `http://localhost:5177`。
2. 添加文本、URL、PDF、图片、音频或视频数据源。
3. 在问答面板中提出问题。
4. 查看答案和引用信息。
5. 在 Embedding 视图中检查数据源点和查询点。

## API

| 方法 | Endpoint | 说明 |
| --- | --- | --- |
| `GET` | `/health` | 后端状态、ADK 可用性、Provider、维度和数据源数量 |
| `GET` | `/space` | 当前数据源、投影点、事件轨迹和投影元数据 |
| `POST` | `/sources/text` | 添加文本数据源 |
| `POST` | `/sources/url` | 获取并索引公开 URL |
| `POST` | `/sources/file` | 上传并索引 PDF、图片、音频或视频 |
| `DELETE` | `/sources/{source_id}` | 删除数据源及其文本块 |
| `POST` | `/ask` | 检索证据、运行 ADK 回答流程并返回引用信息 |

## 注意事项

- 数据存储在内存中。重启后端会重置演示索引。
- URL 摄取默认阻止 localhost 和私有 IP 地址范围，除非设置 `ALLOW_PRIVATE_URLS=true`。
- 通过 Gemini File API 上传的媒体文件会在生成 Embedding 后被清理。
- 阻塞式媒体处理会在线程池中运行，因此不会阻塞 FastAPI 事件循环。
- 如果用于生产环境，应将内存存储替换为持久化存储，并增加身份验证、后台摄取、评估、可观测性以及托管向量数据库。
