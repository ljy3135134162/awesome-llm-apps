# Contextual AI RAG Agent

这是一个集成 Contextual AI 托管式 RAG 平台的 Streamlit 应用。你可以创建数据存储、导入文档、启动 Agent，并基于自己的数据进行有依据的对话。

## 功能

- 将文档导入 Contextual AI 数据存储
- 创建绑定到一个或多个数据存储的 Agent
- 通过 Contextual 的 Grounded Language Model（GLM）生成响应，提供忠实且基于检索内容的答案
- 根据查询相关性和自定义指令对检索文档进行重排序（支持多语言）
- 检索结果可视化（展示引用页面图像和元数据）
- 使用自定义评分标准，通过 LMUnit 对回答进行评估

## 环境要求

- Contextual AI 账户和 API Key（Dashboard → API Keys）

### 生成 API Key

1. 登录你的租户：`app.contextual.ai`。
2. 点击“API Keys”。
3. 点击“Create API Key”。
4. 复制生成的密钥，并在应用提示时粘贴到侧边栏中。

## 运行方法

1. 克隆仓库并进入应用目录：
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/rag_tutorials/contextualai_rag_agent
```

2. 创建并激活虚拟环境。
3. 安装依赖：
```bash
pip install -r requirements.txt
```
4. 启动应用：
```bash
streamlit run contextualai_rag_agent.py
```

## 使用方法

1. 在侧边栏粘贴 Contextual AI API Key。如果你已经有现成的 Agent ID 和/或 Datastore ID，也可以选择填写。

2. 如有需要，创建新的数据存储。上传 PDF 或文本文件进行导入。应用会等待文档处理完成。

3. 创建新的 Agent（或使用已有 Agent），并将其关联到数据存储。

4. 在聊天输入框中提问。响应将由你的 Contextual AI Agent 生成。

5. 可选高级功能：
   - **Agent Settings**：通过 UI 更新 Agent 的系统提示词。
   - **Debug & Evaluation**：开启检索信息以查看引用来源；使用自定义评分标准，通过 LMUnit 对上一条回答进行评估。

## 配置说明

- 如果你使用的是非美国区域的云实例，请在侧边栏设置 Base URL（例如 `http://api.contextual.ai/v1`）。应用会将该 Base URL 用于所有 API 调用，包括就绪状态轮询。
- 检索可视化通过 `agents.query.retrieval_info` 获取 Base64 编码的页面图像，并直接进行展示。
- LMUnit 评估通过 `lmunit.create`，按照你的评分标准对上一条回答进行打分。
