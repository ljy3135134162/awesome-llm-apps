# PharmaQuery

## 概述
PharmaQuery 是一个高级制药洞察检索系统，旨在帮助用户从制药领域的研究论文和文档中获取有价值的信息与洞察。

## 演示
https://github.com/user-attachments/assets/c12ee305-86fe-4f71-9219-57c7f438f291

## 功能
- **自然语言查询**：针对制药行业提出复杂问题，并获得简洁、准确的回答。
- **自定义数据库**：上传自己的研究文档，以扩展检索系统的知识库。
- **相似度搜索**：使用 AI Embedding 检索与查询最相关的文档。
- **Streamlit 界面**：提供易于使用的查询和文档上传界面。

## 使用的技术
- **编程语言**：[Python 3.10+](https://www.python.org/downloads/release/python-31011/)
- **框架**：[LangChain](https://www.langchain.com/)
- **数据库**：[ChromaDB](https://www.trychroma.com/)
- **模型**：
  - Embedding：[Google Gemini API（embedding-001）](https://ai.google.dev/gemini-api/docs/embeddings)
  - Chat：[Google Gemini API（gemini-1.5-pro）](https://ai.google.dev/gemini-api/docs/models/gemini#gemini-1.5-pro)
- **PDF 处理**：[PyPDFLoader](https://python.langchain.com/docs/integrations/document_loaders/pypdfloader/)
- **文档切分器**：[SentenceTransformersTokenTextSplitter](https://python.langchain.com/api_reference/text_splitters/sentence_transformers/langchain_text_splitters.sentence_transformers.SentenceTransformersTokenTextSplitter.html)

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
   - 在主界面输入查询内容。
   - 如有需要，可在侧边栏上传研究论文，以增强数据库中的知识内容。

## :mailbox: 联系我
<img align="right" src="https://media.giphy.com/media/2HtWpp60NQ9CU/giphy.gif" alt="handshake gif" width="150">

<p align="left">
  <a href="https://linkedin.com/in/codewithcharan" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="codewithcharan" height="30" width="40" style="margin-right: 10px" /></a>
  <a href="https://instagram.com/joyboy._.ig" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/instagram.svg" alt="__mr.__.unique" height="30" width="40" /></a>
  <a href="https://twitter.com/Joyboy_x_" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/twitter.svg" alt="codewithcharan" height="30" width="40" style="margin-right: 10px" /></a>
</p>