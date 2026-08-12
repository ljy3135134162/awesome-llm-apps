# 使用 Cohere Embed-4 的 Vision RAG 🖼️

一个强大的视觉检索增强生成（RAG）系统，使用 Cohere 先进的 Embed-4 模型进行多模态 Embedding，并使用 Google 高效的 Gemini 2.5 Flash 模型回答有关图片和 PDF 页面的提问。

## 功能

- **多模态搜索**：利用 Cohere Embed-4，根据文本问题查找语义上最相关的图片（或 PDF 页面图片）。
- **视觉问答**：使用 Google Gemini 2.5 Flash 分析检索到的图片/页面内容，并生成准确、具备上下文感知能力的答案。
- **灵活的内容来源**：
    - 使用预加载的示例财务图表和信息图。
    - 上传自己的图片（PNG、JPG、JPEG）。
    - **上传 PDF 文档**：自动将页面提取为图片进行分析。
- **无需 OCR**：直接处理 PDF 页面中的复杂图片和视觉元素，无需单独执行文本提取步骤。
- **交互式 UI**：使用 Streamlit 构建，可方便地加载内容、输入问题并查看结果。
- **会话管理**：在当前会话中保留已加载/上传的内容，包括图片和处理后的 PDF 页面。

## 环境要求

- Python 3.8+
- Cohere API Key
- Google Gemini API Key

## 如何运行

按照以下步骤配置并运行应用：

1. **克隆仓库并进入目录**：
    ```bash
    git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
    cd awesome-llm-apps/rag_tutorials/vision_rag
    ```

2. **安装依赖**：
    ```bash
    pip install -r requirements.txt
    ```
    *（请确保已安装最新版 `PyMuPDF` 以及其他依赖。）*

3. **配置 API Key**：
    - 从 [Cohere Platform](https://dashboard.cohere.com/api-keys) 获取 Cohere API Key。
    - 从 [Google AI Studio](https://aistudio.google.com/app/apikey) 获取 Google API Key。

4. **运行 Streamlit 应用**：
    ```bash
    streamlit run vision_rag.py
    ```

5. **访问 Web 界面**：
    - Streamlit 会在终端中提供一个本地 URL，通常为 `http://localhost:8501`。
    - 在浏览器中打开该地址。

## 工作原理

应用采用两阶段 RAG 流程：

1. **检索（Retrieval）**：
    - 加载示例图片或上传自己的图片/PDF 时：
        - 普通图片会被转换为 Base64 字符串。
        - **PDF 会逐页处理**：每一页都会渲染成图片、临时保存，然后转换为 Base64 字符串。
    - 使用 Cohere `embed-v4.0` 模型（`input_type="search_document"`）为每张图片或 PDF 页面图片生成稠密向量 Embedding。
    - 用户提出问题后，文本查询使用同一个 `embed-v4.0` 模型（`input_type="search_query"`）生成 Embedding。
    - 计算问题 Embedding 与所有图片 Embedding 之间的余弦相似度。
    - 检索相似度最高的图片作为最相关上下文；它可能是普通图片，也可能是 PDF 中的某一页。

2. **生成（Generation）**：
    - 将原始文本问题以及检索得到的图片/页面图片一并传给 Google `gemini-2.5-flash-preview-04-17` 模型。
    - Gemini 根据问题上下文分析图片内容，并生成文本答案。

## 使用方法

1. 在侧边栏输入 Cohere 和 Google API Key。
2. 加载内容：
    - 点击 **“Load Sample Images”** 下载并处理内置示例图片。
    - 或者/同时使用 **“Upload Your Images or PDFs”** 上传自己的图片或 PDF 文件。
3. 内容加载并处理完成（Embedding 已生成）后，**“Ask a Question”** 区域将被启用。
4. 可以展开 **“View Loaded Images”**，查看当前会话中全部图片以及已处理 PDF 页面的缩略图。
5. 在文本输入框中输入关于已加载内容的问题。
6. 点击 **“Run Vision RAG”**。
7. 查看结果：
    - **Retrieved Image/Page**：系统判断与问题最相关的图片/页面；如果来自 PDF，标题会标明源 PDF 和页码。
    - **Generated Answer**：Gemini 根据图片和问题生成的答案。

## 使用场景

- 分析财务图表并提取关键数字或趋势。
- 针对图片或 PDF 中的示意图、流程图和信息图进行问答。
- 无需显式 OCR，即可从截图或 PDF 页面中的表格和文本提取信息。
- 使用自然语言构建和查询由图片、PDF 组成的视觉知识库。
- 理解包括多页报告在内的复杂视觉文档。

## 注意事项

- 图片和 PDF 处理（页面渲染 + Embedding）可能需要一定时间，尤其是在文件数量较多或文件较大时。示例图片首次加载后会被缓存；当前 PDF 会在每次会话上传时重新处理。
- 请确保 API Key 拥有调用 Cohere 和 Gemini 对应模型所需的权限与配额。
- 最终回答质量同时取决于检索图片的相关性，以及 Gemini 根据问题理解图片内容的能力。
