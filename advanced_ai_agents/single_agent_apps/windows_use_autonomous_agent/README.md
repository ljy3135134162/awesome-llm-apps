<div align="center">

  <h1>🪟 Windows Use 自主 Agent</h1>

  <a href="https://github.com/CursorTouch/windows-use/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  </a>
  <img src="https://img.shields.io/badge/python-3.12%2B-blue" alt="Python">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011-blue" alt="Platform">
  <br>

  <a href="https://x.com/CursorTouch">
    <img src="https://img.shields.io/badge/follow-%40CursorTouch-1DA1F2?logo=twitter&style=flat" alt="Follow on Twitter">
  </a>
  <a href="https://discord.com/invite/Aue9Yj2VzS">
    <img src="https://img.shields.io/badge/Join%20on-Discord-5865F2?logo=discord&logoColor=white&style=flat" alt="Join us on Discord">
  </a>

</div>

<br>

**Windows-Use** 是一个强大的自动化 Agent，可直接在 Windows GUI 层进行交互。它连接 AI Agent 与 Windows 操作系统，可以执行打开应用、点击按钮、输入文字、运行 Shell 命令、捕获 UI 状态等任务，并且不依赖传统计算机视觉模型。这样可以让任意 LLM 具备电脑自动化能力，而不必依赖专用的计算机操作模型。

## 🛠️ 安装指南

### 前置条件

- Python 3.12 或更高版本
- [UV](https://github.com/astral-sh/uv)（也可使用 `pip`）
- Windows 10 或 Windows 11

### 安装步骤

**使用 `uv` 安装：**

```bash
uv pip install windows-use
```

或者使用 pip：

```bash
pip install windows-use
```

## ⚙️ 基本用法

```python
# main.py
from langchain_google_genai import ChatGoogleGenerativeAI
from windows_use.agent import Agent
from dotenv import load_dotenv

load_dotenv()

llm=ChatGoogleGenerativeAI(model='gemini-2.0-flash')
agent = Agent(llm=llm,use_vision=True)
query=input("Enter your query: ")
agent_result=agent.invoke(query=query)
print(agent_result.content)
```

## 🤖 运行 Agent

可以通过以下方式从脚本中运行：

```bash
python main.py
Enter your query: <YOUR TASK>
```

---

## 🎥 演示

**提示词：** 写一段关于 LLM 的简短说明，并保存到桌面。

<https://github.com/user-attachments/assets/0faa5179-73c1-4547-b9e6-2875496b12a0>

**提示词：** 将系统从深色模式切换为浅色模式。

<https://github.com/user-attachments/assets/47bdd166-1261-4155-8890-1b2189c0a3fd>

## 愿景

直接和电脑对话，然后看着它完成任务。

## 路线图

### 🤖 Agent 智能能力

* [ ] **集成记忆能力**：让 Agent 能够记住用户之前的交互。
* [ ] **优化 Token 使用量**：使用 Ally Tree 压缩、提示词工程等方式降低上下文开销。
* [ ] **模拟更高级的人类输入行为**：在不同应用中实现更准确、自然的鼠标和键盘操作。
* [ ] **支持本地 LLM**：让本地模型在自动化任务中的表现尽可能接近云端 API，例如 Mistral、LLaMA 等。
* [ ] **改进推理与规划能力**：增强 Agent 对复杂任务进行拆解和排序执行的能力。

### 🌳 Ally Tree 优化

* [ ] **改进 UI 元素检测**：自动识别并优先保留屏幕中关键、可交互的组件。
* [ ] **智能压缩 Ally Tree**：通过裁剪无关分支降低结构复杂度。
* [ ] **上下文相关的优先级排序**：根据当前任务的重要程度对 UI 元素进行排序。

### 💡 用户体验

* [ ] **降低延迟**：优化 GUI 交互之间的响应速度。
* [ ] **优化命令界面**：通过更简单的 UX 层让用户更方便地输入、编写或语音下达命令。
* [ ] **增强错误处理和恢复能力**：更稳健地处理边缘情况和不明确的指令。

### 🧪 评估

* [ ] **LLM 评测基准**：跟踪不同模型在各类 Benchmark 上的表现。

## ⚠️ 注意事项

该 Agent 会直接在 Windows GUI 层执行操作。虽然其设计目标是尽可能智能且安全地完成任务，但仍有可能出现误操作，导致不期望的系统行为或文件变更。建议优先在沙盒或隔离环境中运行。

由 [Jeomon George](https://github.com/Jeomon) 制作 ❤️

---

## 引用

```bibtex
@software{
  author       = {George, Jeomon},
  title        = {Windows-Use: Enable AI to control Windows OS},
  year         = {2025},
  publisher    = {GitHub},
  url={https://github.com/CursorTouch/Windows-Use}
}
```
