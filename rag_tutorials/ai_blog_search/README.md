# 使用 LangGraph 构建 Agentic RAG：AI 博客搜索

## 概述
AI Blog Search 是一个 Agentic RAG 应用，旨在增强对 AI 相关博客文章的信息检索能力。该系统利用 LangChain、LangGraph 和 Google Gemini 模型获取、处理和分析博客内容，为用户提供准确且符合上下文的答案。

## LangGraph 工作流
![LangGraph-Workflow](https://github.com/user-attachments/assets/07d8a6b5-f1ef-4b7e-b47a-4f14a192bd8a)

## 演示
https://github.com/user-attachments/assets/cee07380-d3dc-45f4-ad26-7d944ba9c32b

## 功能
- **文档检索：** 使用 Qdrant 作为向量数据库，根据嵌入向量存储和检索博客内容。
- **Agentic 查询处理：** 使用 AI Agent 判断查询应该被重写、直接回答，还是需要进一步检索。
- **相关性评估：** 使用 Google Gemini 模型实现自动相关性评分系统。
- **查询优化：** 优化结构较差的查询，以获得更好的检索结果。
- **Streamlit UI：** 提供易用界面，用于输入博客 URL、查询问题并获取有价值的响应。
- **基于图的工作流：** 使用 LangGraph 实现结构化状态图，以提高决策效率。

## 使用的技术
- **编程语言**：[Python 3.10+](https://www.python.org/downloads/release/python-31011/)
- **框架**：[LangChain](https://www.langchain.com/) 和 [LangGraph](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
- **数据库**：[Qdrant](https://qdrant.tech/)
- **模型**：
  - Embedding：[Google Gemini API（embedding-001）](https://ai.google.dev/gemini-api/docs/embeddings)
  - Chat：[Google Gemini API（gemini-2.0-flash）](https://ai.google.dev/gemini-api/docs/models/gemini#gemini-2.0-flash)
- **博客加载器**：[LangChain WebBaseLoader](https://python.langchain.com/docs/integrations/document_loaders/web_base/)
- **文档切分器**：[RecursiveCharacterTextSplitter](https://python.langchain.com/v0.1/docs/modules/data_connection/document_transformers/recursive_text_splitter/)
- **用户界面（UI）**：[Streamlit](https://docs.streamlit.io/)

## 使用要求
1. **安装依赖**：
   ```bash
   pip install -r requirements.txt
   ```

2. **运行应用**：
   ```bash
   streamlit run app.py
   ```

3. **使用应用**：
   - 在侧边栏粘贴 Google API Key。
   - 粘贴博客链接。
   - 输入你希望针对该博客文章提出的问题。

## :mailbox: 联系我
<img align="right" src="https://media.giphy.com/media/2HtWpp60NQ9CU/giphy.gif" alt="handshake gif" width="150">

<p align="left">
  <a href="https://linkedin.com/in/codewithcharan" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="codewithcharan" height="30" width="40" style="margin-right: 10px" /></a>
  <a href="https://instagram.com/joyboy._.ig" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/instagram.svg" alt="__mr.__.unique" height="30" width="40" /></a>
  <a href="https://twitter.com/Joyboy_x_" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/twitter.svg" alt="codewithcharan" height="30" width="40" style="margin-right: 10px" /></a>
</p>

<img src="https://readme-typing-svg.herokuapp.com/?font=Righteous&size=35&center=true&vCenter=true&width=500&height=70&duration=4000&lines=Thanks+for+visiting!+👋;+Message+me+on+Linkedin!;+I'm+always+down+to+collab+:)"/>
