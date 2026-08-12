## 🦙💬 使用 Llama-3 构建 ChatGPT 克隆

这个项目演示了如何使用在本地计算机上运行的 Llama-3 模型构建一个 ChatGPT 克隆。应用使用 Python 和 Streamlit 开发，为用户提供易于使用的语言模型交互界面。更重要的是，它完全免费，并且不需要互联网连接！

### 功能

- 完全在本地计算机上运行，无需互联网连接，并且可以免费使用
- 使用 Llama-3 Instruct 模型生成回答
- 提供类似聊天应用的界面，实现流畅交互

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_tools_frameworks/local_chatgpt_clone
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 下载并安装 [LM Studio 桌面应用](https://lmstudio.ai/)，然后下载 Llama-3 Instruct 模型。

4. 在 LM Studio 中启动服务器，将 Llama-3 模型以兼容 OpenAI API 的形式提供服务。可以参考这个[视频教程](https://x.com/Saboo_Shubham_/status/1783715814790549683)。

5. 运行 Streamlit 应用

```bash
streamlit run chatgpt_clone_llama3.py
```
