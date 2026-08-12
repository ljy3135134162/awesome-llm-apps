## 📝 与 Substack Newsletter 对话

这是一个 Streamlit 应用，可以使用 OpenAI API 和 Embedchain 库与 Substack Newsletter 进行对话。应用利用 GPT-4，根据指定 Substack Newsletter 的内容为用户问题提供准确回答。

## 功能

- 输入 Substack 博客 URL
- 针对 Substack Newsletter 的内容提出问题
- 使用 OpenAI API 和 Embedchain 获取准确回答

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/chat_with_X_tutorials/chat_with_substack
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI 账户](https://platform.openai.com/)（也可以使用你选择的其他 LLM 提供商），并获取 API Key。

4. 运行 Streamlit 应用

```bash
streamlit run chat_substack.py
```
