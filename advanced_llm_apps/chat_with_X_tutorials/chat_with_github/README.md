## 💬 与 GitHub 仓库对话

这是一个仅用约 30 行 Python 代码实现的 GitHub 仓库 RAG 对话应用。应用使用检索增强生成（RAG），根据指定 GitHub 仓库中的内容，为用户问题提供准确回答。

### 功能

- 输入 GitHub 仓库名称
- 针对 GitHub 仓库内容提出问题
- 使用 OpenAI API 和 Embedchain 获取基于仓库内容的回答

### 如何开始？

1. 克隆 GitHub 仓库

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_llm_apps/chat_with_X_tutorials/chat_with_github
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册 [OpenAI 账户](https://platform.openai.com/)（也可以使用你选择的其他 LLM 提供商），并获取 API Key。

4. 获取 GitHub Access Token

- 创建一个具有访问目标 GitHub 仓库所需权限的 [Personal Access Token](https://docs.github.com/en/enterprise-server@3.6/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token)。

5. 运行 Streamlit 应用

```bash
streamlit run chat_github.py
```

### 工作原理

- 应用会提示用户输入 OpenAI API Key，用于对 OpenAI API 请求进行身份验证。

- 使用提供的 GitHub Access Token 初始化 Embedchain App 实例和 `GithubLoader`。

- 用户输入 GitHub 仓库 URL，应用随后通过 `GithubLoader` 将该仓库内容加入 Embedchain 的知识库。

- 用户可以通过文本输入框针对 GitHub 仓库提出问题。

- 当用户提出问题时，应用调用 Embedchain App 的 `chat` 方法，根据 GitHub 仓库内容生成回答。

- 最后，应用将生成的回答展示给用户。
