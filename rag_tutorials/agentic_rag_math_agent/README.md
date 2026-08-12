# 🧠 数学导师 Agent——带反馈闭环的 Agentic RAG

本项目实现了一套 **Agentic RAG 架构**，用于模拟数学教授，为 **JEE 级别的数学问题**提供分步骤讲解。系统能够智能地在向量数据库和 Web 搜索之间路由查询，应用输入/输出 Guardrail，并引入人工反馈以实现持续学习。

## 📌 功能

- ✅ **输入 Guardrail**（DSPy）：仅接受学术类数学问题。
- 📚 **知识库搜索**：使用带 OpenAI Embedding 的 **Qdrant 向量数据库**匹配已知问题。
- 🌐 **Web 回退搜索**：找不到高质量匹配时调用 **Tavily API**。
- ✍️ **GPT-4.1 解题说明**：生成分步骤的数学解答。
- 🛡️ **输出 Guardrail**：对输出进行正确性和安全性过滤。
- 👍 **Human-in-the-Loop 反馈**：用户可以对答案进行 Yes/No 评价，并记录下来用于后续学习。
- 📊 **基准测试**：使用 **JEEBench** 数据集进行评估，并可调整测试题目数量。
- 💻 **Streamlit UI**：提供带多个标签页的交互式仪表板。

## 🚀 架构流程
<img width="465" alt="Screenshot 2025-05-04 at 3 45 58 PM" src="https://github.com/user-attachments/assets/c0a9e612-2ef0-413c-b779-c99fe9f48619" />

## 📚 知识库

- **数据集：** [JEEBench（HuggingFace）](https://huggingface.co/datasets/daman1209arora/jeebench)
- **向量数据库：** Qdrant（使用 OpenAI Embedding）
- **存储：** 使用 `llama-index` 持久化嵌入向量，并执行 Top-1 相似度搜索

## 🌐 Web 搜索

- 当知识库中没有合适匹配时，使用 **Tavily API** 进行回退搜索
- 获取到的内容会传入 **GPT-4o**，生成清晰的解题说明

## 🔐 Guardrail

- **输入 Guardrail（DSPy）：** 仅接受与数学相关的学术问题
- **输出 Guardrail（DSPy）：** 阻止幻觉内容或偏离主题的输出

## 👨‍🏫 Human-in-the-Loop 反馈

- Streamlit UI 允许学生在查看答案后给出 👍 / 👎 反馈
- 反馈会记录到本地 JSON 文件中，用于后续改进

## 📊 基准测试

- 使用 **50 道随机 JEEBench 数学题**进行评估
- **当前准确率：** 66%
- 基准测试结果保存至：`benchmark/results.csv`

## 🚀 演示

使用 Streamlit 运行应用：

```bash
streamlit run app/streamlit.py
```
