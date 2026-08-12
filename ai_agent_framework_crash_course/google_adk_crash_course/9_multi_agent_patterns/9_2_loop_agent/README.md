# 🔁 教程 9.2：Loop Agent——迭代式计划优化器

## 🎯 你将学到什么

- **Loop Agent 组合**：在循环中按顺序执行多个子 Agent
- **有状态迭代**：在多轮迭代之间持久化计数器和标志
- **终止条件**：达到最大迭代次数，或由子 Agent 触发升级后停止
- **Streamlit Web 界面**：通过交互式 UI 执行迭代优化

## 🧠 核心概念：带条件控制的 LoopAgent

根据 ADK 工作流 Agent 文档，**LoopAgent** 会重复执行一组子 Agent，并在不同迭代之间共享相同的上下文和状态。本教程实现一个**迭代式计划优化器**，它会经过多轮迭代不断改进计划，并在满足指定条件后停止。

```text
主题 → LoopAgent → [优化计划] → [增加迭代次数] → [检查完成条件]
       ↑                                                    │
       └────────────────── 重复直到停止 ────────────────────┘
```

**终止机制**：当达到可选的 `max_iterations`，或者任意子 Agent 返回一个 `EventActions` 中带有 `escalate=True` 的 `Event` 时，循环都会停止。

**上下文与状态**：所有迭代共享同一个 `InvocationContext` 和 `session.state`，因此 `iteration`、`target_iterations`、`accepted` 等值能够持续存在，并用于控制循环。

## 📁 项目结构

```text
9_2_loop_agent/
├── agent.py              # 包含 3 个子 Agent 和 Session State 控制的 LoopAgent
├── app.py                # 用于运行循环优化的 Streamlit UI
└── README.md             # 本文档
```

## 🚀 快速开始

### 1. 安装依赖
```bash
cd 9_2_loop_agent
pip install -r ../9_1_sequential_agent/requirements.txt
```

### 2. 配置环境
创建包含 Google API Key 的 `.env` 文件，也可以复用顺序 Agent 示例中的配置：

```bash
echo "GOOGLE_API_KEY=your_ai_studio_key_here" > .env
```

### 3. 运行 Streamlit 应用
```bash
streamlit run app.py
```

## 🧪 工作原理

- **plan_refiner（LlmAgent）**：每轮迭代生成一个更精炼、更完善的计划。
- **increment_iteration（BaseAgent）**：增加 `session.state['iteration']` 的值。
- **check_completion（BaseAgent）**：当 `accepted=True` 或 `iteration >= target_iterations` 时触发升级并终止循环。

`LoopAgent` 会在每轮迭代中依次执行这些子 Agent，并持续保存和更新状态，直到满足停止条件。

### Session State 关键字段

- **topic**：当前正在优化的主题。
- **iteration**：当前迭代次数。
- **target_iterations**：循环停止前允许执行的目标迭代次数。
- **accepted**：设置为 `True` 时立即终止循环。

## 🧪 尝试运行

- 输入一个主题，例如：`AI-powered customer support platform launch plan`。
- 将 `Target iterations` 设置为 3～5。
- 执行后观察最终优化后的计划以及运行元数据。

## 🔧 本教程涉及的 ADK 概念

- 使用顺序子 Agent 的 **LoopAgent 模式**
- 跨迭代的 **Session State 持久化**
- 使用 `EventActions(escalate=True)` 实现**基于升级机制的终止控制**
- **Runner + SessionService** 执行模式

## 🔎 故障排查

- 确认 `.env` 中已正确设置 `GOOGLE_API_KEY`。
- 从包含 `app.py` 的目录运行应用。
- 如果之前运行过应用，将继续复用相同的 Session ID；修改主题或目标迭代次数时，对应状态也会随之更新。

## 📚 关键要点

- **LoopAgent** 适合构建需要反复优化的迭代式工作流。
- **共享状态**允许复杂的控制信号在不同迭代之间持续累积。
- 将不同职责拆分为**清晰、模块化的子 Agent**，可以让循环控制逻辑更容易理解和维护。
