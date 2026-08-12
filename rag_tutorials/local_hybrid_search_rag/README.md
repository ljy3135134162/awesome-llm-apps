# 🖥️ 使用混合搜索的本地 RAG 应用

一个强大的文档问答应用，结合混合搜索（Hybrid Search / RAG）与本地 LLM，提供全面的回答。该系统使用 RAGLite 实现可靠的文档处理和检索，并使用 Streamlit 构建直观的聊天界面，将文档专属知识与本地 LLM 能力相结合，从而生成准确且具备上下文的响应。

## 演示

https://github.com/user-attachments/assets/375da089-1ab9-4bf4-b6f3-733f44e47403

## 快速开始

如需立即测试，可使用以下已验证的模型配置：

```bash
# LLM 模型
bartowski/Llama-3.2-3B-Instruct-GGUF/Llama-3.2-3B-Instruct-Q4_K_M.gguf@4096

# Embedding 模型
lm-kit/bge-m3-gguf/bge-m3-Q4_K_M.gguf@1024
```

这些模型在性能和资源占用之间取得了较好的平衡，并且已经验证，即使在配备 8GB 内存的 MacBook Air M2 上也能很好地配合运行。

## 功能

- **本地 LLM 集成**：
  - 使用 llama-cpp-python 模型进行本地推理
  - 支持多种量化格式（推荐 Q4_K_M）
  - 可配置上下文窗口大小

- **文档处理**：
  - PDF 文档上传和处理
  - 自动文本分块和 Embedding
  - 结合语义匹配与关键词匹配的混合搜索
  - 通过重排序获得更优的上下文选择

- **多模型集成**：
  - 使用本地 LLM 进行文本生成（例如 Llama-3.2-3B-Instruct）
  - 使用 BGE 模型进行本地 Embedding
  - 使用 FlashRank 进行本地重排序

## 环境要求

1. **安装 spaCy 模型**：
   ```bash
   pip install https://github.com/explosion/spacy-models/releases/download/xx_sent_ud_sm-3.7.0/xx_sent_ud_sm-3.7.0-py3-none-any.whl
   ```

2. **安装加速版 llama-cpp-python**（可选但推荐）：
   ```bash
   # 配置安装变量
   LLAMA_CPP_PYTHON_VERSION=0.3.2
   PYTHON_VERSION=310 # 3.10、3.11、3.12
   ACCELERATOR=metal  # Mac
   # ACCELERATOR=cu121  # NVIDIA GPU
   PLATFORM=macosx_11_0_arm64  # Mac
   # PLATFORM=linux_x86_64  # Linux
   # PLATFORM=win_amd64  # Windows

   # 安装加速版本
   pip install "https://github.com/abetlen/llama-cpp-python/releases/download/v$LLAMA_CPP_PYTHON_VERSION-$ACCELERATOR/llama_cpp_python-$LLAMA_CPP_PYTHON_VERSION-cp$PYTHON_VERSION-cp$PYTHON_VERSION-$PLATFORM.whl"
   ```

3. **安装依赖**：
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd awesome-llm-apps/rag_tutorials/local_hybrid_search_rag
   pip install -r requirements.txt
   ```

## 模型配置

RAGLite 在 LiteLLM 的基础上扩展了对 llama.cpp 模型的支持，并通过 llama-cpp-python 运行这些模型。要选择 llama.cpp 模型（例如 bartowski 的模型集合），请使用如下格式的模型标识符：`llama-cpp-python/<hugging_face_repo_id>/<filename>@<n_ctx>`，其中 `n_ctx` 是可选参数，用于指定模型上下文长度。

1. **LLM 模型路径格式**：
   ```
   llama-cpp-python/<repo>/<model>/<filename>@<context_length>
   ```
   示例：
   ```
   bartowski/Llama-3.2-3B-Instruct-GGUF/Llama-3.2-3B-Instruct-Q4_K_M.gguf@4096
   ```

2. **Embedding 模型路径格式**：
   ```
   llama-cpp-python/<repo>/<model>/<filename>@<dimension>
   ```
   示例：
   ```
   lm-kit/bge-m3-gguf/bge-m3-Q4_K_M.gguf@1024
   ```

## 数据库配置

应用支持多种数据库后端：

- **PostgreSQL**（推荐）：
  - 可在 [Neon](https://neon.tech) 中快速创建免费的 Serverless PostgreSQL 数据库
  - 支持即时创建和缩容至零（scale-to-zero）
  - 连接字符串格式：`postgresql://user:pass@ep-xyz.region.aws.neon.tech/dbname`

## 如何运行

1. **启动应用**：
   ```bash
   streamlit run local_main.py
   ```

2. **配置应用**：
   - 输入 LLM 模型路径
   - 输入 Embedding 模型路径
   - 设置数据库 URL
   - 点击“Save Configuration”

3. **上传文档**：
   - 通过界面上传 PDF 文件
   - 等待处理完成

4. **开始聊天**：
   - 针对文档内容提出问题
   - 使用本地 LLM 获取回答
   - 在需要时回退到通用知识回答

## 说明

- 大多数使用场景推荐使用 4096 的上下文窗口
- Q4_K_M 量化在速度与质量之间具有较好的平衡
- 1024 维的 BGE-M3 Embedding 模型表现较优
- 本地模型需要足够的 RAM 和 CPU/GPU 资源
- Mac 可使用 Metal 加速，NVIDIA GPU 可使用 CUDA 加速

## 贡献

欢迎贡献！可以随时提交 Pull Request。
