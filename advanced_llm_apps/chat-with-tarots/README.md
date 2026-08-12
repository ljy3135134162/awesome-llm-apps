# ✨ The Magician IA Reader：AI 驱动的 NLP 与塔罗解读 ✨

欢迎使用 **The Magician IA Reader**！这个项目将人工智能的能力与塔罗占卜的神秘体系结合起来，提供一种独特的 AI 应用体验。

![TheMagicianDemo](https://github.com/maurizioorani/TheMagician-IA-Reader/blob/main/data/readme/TheMagicianAI.gif)

## 功能说明

该应用可以作为一个 AI 塔罗解读器使用。用户输入自然语言问题后，系统会结合传统塔罗牌含义，由 AI 模型生成相应的解释和洞察。

## 核心特性

- **自然语言支持**：可以理解并使用自然语言进行交互，目前默认配置为英语。
- **本地 AI 模型（`phi4`）**：基于高效的 `phi4` 模型本地运行，更适合隐私敏感场景。
- **CSV 驱动的知识库**：使用结构化 CSV 文件保存并查询详细的塔罗牌含义与象征信息，目前使用包含英文内容的 `data/tarots.csv`。
- **深度解读**：将原始文本问题转换为结合上下文和塔罗象征体系的解释结果。

## 工作原理

The Magician IA Reader 的核心是本地运行的 `phi4` AI 模型。模型通过包含每张塔罗牌解释信息的 CSV 数据进行提示或知识增强。

当用户输入一段文本后，应用会将内容交给 AI 模型处理，再结合塔罗牌含义生成对应回答。

## 为什么使用它？

- **研究人员和开发者**：研究本地 AI 模型在自然语言理解与生成方面的能力。
- **AI 爱好者**：体验 AI 在特殊垂直领域中的实际应用方式。
- **感兴趣的普通用户**：通过一种不同的方式体验 AI 与个人解读之间的结合。

通过 The Magician IA Reader，体验 AI 与直觉体系结合的应用方式。

---

## ⚙️ 安装

### 环境要求

- **Python 3.8 或更高版本**
- **pip**：Python 包管理工具
- **Ollama**：需要在本地运行

从 https://ollama.com/ 安装后执行：

```bash
ollama pull phi4
ollama serve
```

### 安装步骤

1. **克隆仓库**

```bash
git clone https://github.com/maurizioorani/TheMagician-IA-Reader.git
cd TheMagician-IA-Reader
```

2. **创建虚拟环境（推荐）**

```bash
# Unix/macOS：
python -m venv venv
source venv/bin/activate

# Windows：
python -m venv venv
venv\Scripts\activate
```

## 前端（Streamlit）

首先安装全部依赖：

```bash
pip install -r requirements.txt
```

运行应用：

```bash
streamlit run app.py
```

Streamlit 界面通常可以通过 `http://localhost:8501` 访问。

## 📖 使用方法

1. **打开应用**：在浏览器中访问 Streamlit 提供的地址，通常为 `http://localhost:8501`。
2. **输入文本内容**：通过界面粘贴、输入或上传需要解答的问题。
3. **选择抽牌数量**：可以选择使用 3、5 或 7 张牌进行解读。
4. **查看解读结果**：查看系统生成的详细分析和可视化结果，了解抽取牌面的含义。

## 🤝 贡献

欢迎贡献改进或新增功能。如果希望参与：

1. Fork 本仓库。
2. 为修改创建新的分支。
3. 提交 Pull Request，并清楚说明修改内容。

如果计划进行较大的改动，请先通过 Issue 进行讨论。
