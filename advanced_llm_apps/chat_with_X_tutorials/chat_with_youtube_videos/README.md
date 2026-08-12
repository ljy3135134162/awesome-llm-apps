## 📽️ 与 YouTube 视频对话

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-an-llm-app-with-rag-to-chat-with-youtube-videos)，通过详细的代码讲解、说明和最佳实践，从零开始构建这个应用。**

这是一个基于 RAG 的 YouTube 视频对话应用，使用 OpenAI 的 `gpt-4o`、mem0/embedchain 作为记忆组件，并结合 `youtube-transcript-api`。应用通过检索增强生成（RAG），根据视频内容为用户问题提供准确回答。

### 功能

- 输入 YouTube 视频 URL
- 针对视频内容提出问题
- 使用 RAG 和所选择的 LLM 获取准确回答

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/chat_with_X_tutorials/chat_with_youtube_videos
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI 账户](https://platform.openai.com/)（也可以使用你选择的其他 LLM 提供商），并获取 API Key。

4. 运行 Streamlit 应用

```bash
streamlit run chat_youtube.py
```
