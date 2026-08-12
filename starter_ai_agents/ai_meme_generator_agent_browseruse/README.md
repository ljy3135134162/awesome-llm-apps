# 🥸 AI Meme 生成 Agent - Browser Use

AI Meme Generator Agent 是一个基于浏览器自动化的 Meme 生成工具，通过 AI Agent 直接操作网页来创建 Meme。该应用结合多 LLM 能力与自动化浏览器交互，根据文本提示词直接操作网站生成 Meme。

## 功能特性

- **多 LLM 支持**
  - Claude 3.5 Sonnet（Anthropic）
  - GPT-4o（OpenAI）
  - DeepSeek V3（DeepSeek）
  - 根据 API Key 校验结果自动切换模型

- **浏览器自动化**
  - 直接操作 imgflip.com 的 Meme 模板
  - 自动搜索合适的 Meme 格式
  - 自动填写顶部和底部文字
  - 从生成结果中提取图片链接

- **智能生成流程**
  - 从提示词中提取动作语义
  - 根据语义匹配合适的 Meme 模板
  - 多步骤质量校验
  - 生成失败时自动重试

- **易用界面**
  - 模型配置侧边栏
  - API Key 管理
  - Meme 直接预览并提供可点击链接
  - 完善的错误处理

## 所需 API Keys

- **Anthropic**：用于 Claude
- **DeepSeek**
- **OpenAI**：用于 GPT-4o

## 运行方式

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd starter_ai_agents/ai_meme_generator_agent_browseruse
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

如有需要，安装 `playwright` 浏览器运行环境：

```bash
python -m playwright install --with-deps
```

3. **运行 Streamlit 应用**

```bash
streamlit run ai_meme_generator_agent.py
```

### 使用 uv 运行

也可以使用 [uv](https://docs.astral.sh/uv/) 这个高速 Python 包管理器代替 `pip` 运行项目。

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd starter_ai_agents/ai_meme_generator_agent_browseruse
```

2. **安装依赖**

该命令会创建 `.venv`，并根据 `pyproject.toml` / `uv.lock` 安装依赖：

```bash
uv sync
```

如有需要，安装 `playwright` 浏览器二进制文件：

```bash
uv run playwright install --with-deps
```

3. **运行 Streamlit 应用**

```bash
uv run streamlit run ai_meme_generator_agent.py
```
