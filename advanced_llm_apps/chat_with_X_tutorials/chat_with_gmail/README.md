## 📨 与 Gmail 收件箱对话

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-rag-app-to-chat-with-your-gmail-inbox)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个仅用约 30 行 Python 代码实现的 Gmail RAG 对话应用。应用使用检索增强生成（RAG），根据 Gmail 收件箱中的邮件内容，为用户问题提供准确回答。

### 功能

- 连接 Gmail 收件箱
- 针对邮件内容提出问题
- 使用 RAG 和所选 LLM 获取基于邮件内容的准确回答

### 安装

1. 克隆仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_llm_apps/chat_with_X_tutorials/chat_with_gmail
```

2. 安装所需依赖

```bash
pip install -r requirements.txt
```

3. 配置 Google Cloud 项目并启用 Gmail API：

- 前往 [Google Cloud Console](https://console.cloud.google.com/) 并创建新项目。
- 进入 `APIs & Services > OAuth consent screen`，配置 OAuth 同意屏幕。
- 填写所需应用信息并发布 OAuth 同意屏幕。
- 启用 Gmail API，并创建 OAuth Client ID 凭据。
- 下载 JSON 格式的凭据文件，并在工作目录中保存为 `credentials.json`。

4. 获取 OpenAI API Key

- 注册 [OpenAI 账户](https://platform.openai.com/)（也可以使用你选择的其他 LLM 提供商），并获取 API Key。

5. 运行 Streamlit 应用

```bash
streamlit run chat_gmail.py
```
