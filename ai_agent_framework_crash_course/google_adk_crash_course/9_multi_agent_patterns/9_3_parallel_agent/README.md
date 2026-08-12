# ⚡ 教程 9.3：Parallel Agent——市场快照团队

## 🎯 你将学到什么

- **Parallel Agent 组合**：如何并发编排多个专业化 Agent
- **共享状态**：并行子 Agent 如何安全写入同一个 `session.state`
- **分支上下文**：通过 Invocation Branch 隔离工具与记忆上下文
- **Streamlit 界面**：使用简单 UI 运行并展示并行结果

## 🧠 核心概念：使用共享状态的 ParallelAgent

根据 ADK 文档，**Parallel Agent** 会并发执行其子 Agent。每个子 Agent 都运行在独立的 Invocation Branch 上，但共享同一个 `session.state`。

```text
主题 → ParallelAgent → 3 个子 Agent（并发执行）
             ↓
   [市场趋势] + [竞争对手] + [融资新闻]
             ↓
        将快照写入 State
```

每个子 Agent 都将结果写入共享状态中的独立字段，以避免相互覆盖：`market_trends`、`competitors`、`funding_news`。

## 📁 项目结构

```text
9_3_parallel_agent/
├── agent.py              # 并行工作流（3 个研究 Agent + ParallelAgent）
├── app.py                # 用于运行和查看市场快照的 Streamlit UI
├── requirements.txt      # Python 依赖
├── README.md             # 本文档
└── .env.example          # 环境变量示例
```

## 🚀 快速开始

### 1. 安装依赖

```bash
cd 9_3_parallel_agent
pip install -r requirements.txt
```

### 2. 配置环境

创建包含 Google API Key 的 `.env` 文件：

```bash
echo "GOOGLE_API_KEY=your_ai_studio_key_here" > .env
```

> API Key 可从 Google AI Studio 获取。

### 3. 运行 Streamlit 应用

```bash
streamlit run app.py
```

## 🧪 工作原理

- `ParallelAgent` 会并发执行 `market_trends_agent`、`competitor_intel_agent` 和 `funding_news_agent`。
- 每个子 Agent 都使用 Web 搜索，并通过独立的 `output_key` 将结果写入 `session.state`。
- UI 从 `session.state` 读取结果，并以三栏形式展示市场快照。

## 🔧 本教程涉及的 ADK 概念

- `ParallelAgent` 模式与事件交错
- 为每个子 Agent 使用独立字段的共享 `session.state`
- 通过 Invocation Branch 实现上下文隔离
- 使用 Runner 与 Session Service 执行工作流

## 📚 关键要点

- 并行扇出非常适合彼此独立的数据采集任务。
- 为每个子 Agent 设置不同的输出字段，可以避免共享状态中的数据相互覆盖。
- 如果最终需要单一综合报告，可以在并行阶段之后再接一个负责整合结果的下游 Agent。
