# 📄 简历与职位匹配器

## 🚀 概述

这个应用允许你上传一份**简历**和一份**职位描述**，然后使用 LLM：

- ✅ 给出**匹配度评分**（0–100%）
- 💪 突出简历中的优势
- 📝 针对目标职位提出改进建议

对于希望针对每次职位申请优化简历的求职者来说，这是一个非常实用的工具。

---

## 🛠️ 技术栈

- **Python**
- **Streamlit** —— 用于构建用户界面
- **Ollama + LLM**（例如 `llama3`）—— 用于分析
- **PyMuPDF** —— 用于解析 PDF

---

## ⚡ 配置说明

1. 安装依赖：

   ```bash
   pip install -r requirements.txt
   ```

2. 安装 Ollama 并运行一个模型（例如 llama3）：

   ```bash
   ollama run llama3
   ```

3. 启动应用：

   ```bash
   streamlit run app.py
   ```
