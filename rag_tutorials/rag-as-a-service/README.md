## 🖇️ 使用 Claude 3.5 Sonnet 构建 RAG-as-a-Service

使用 Claude 3.5 Sonnet 和 Ragie.ai 构建并部署一套可用于生产环境的检索增强生成（RAG）服务。该实现允许你用不到 50 行 Python 代码创建文档查询系统，并配套一个易用的 Streamlit 界面。

### 功能
- 可用于生产环境的 RAG 流水线
- 集成 Claude 3.5 Sonnet 进行响应生成
- 支持通过 URL 上传文档
- 实时文档查询
- 同时支持快速和高精度两种文档处理模式

### 如何开始？

1. 克隆 GitHub 仓库
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/rag-as-a-service
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 Anthropic API Key 和 Ragie API Key

- 注册 [Anthropic 账户](https://console.anthropic.com/) 并获取 API Key
- 注册 [Ragie 账户](https://www.ragie.ai/) 并获取 API Key

4. 运行 Streamlit 应用
```bash
streamlit run rag_app.py
```