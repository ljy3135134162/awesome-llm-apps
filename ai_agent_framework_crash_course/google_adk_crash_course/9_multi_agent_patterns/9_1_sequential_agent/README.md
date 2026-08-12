# 🎯 教程 9.1：顺序 Agent——业务实施计划生成器

## 🎯 你将学到什么

- **顺序 Agent 组合**：如何按顺序编排多个专业化 Agent
- **AgentTool 集成**：将 Agent 封装为工具以增强能力
- **Web 搜索集成**：通过搜索 Agent 获取实时市场情报
- **业务分析流水线**：从市场调研到实施规划
- **Streamlit Web 界面**：用于业务规划的易用应用界面

## 🧠 核心概念：具备搜索能力的顺序 Agent

根据 [ADK 工作流 Agent 文档](https://google.github.io/adk-docs/agents/workflow-agents/)，**Sequential Agent** 会按顺序依次执行各个子 Agent。本教程实现一个**业务实施计划生成器**，将 Web 搜索与顺序分析流程结合起来：

```text
业务主题 → SequentialAgent → 4 个子 Agent（顺序执行）
                ↓
        [市场调研 + Web 搜索] → [SWOT 分析] → [战略制定] → [实施规划]
                ↓
        完整业务实施计划
```

**核心亮点**：市场调研 Agent 会调用一个专门的搜索 Agent。该搜索 Agent 被封装为 `AgentTool`，从而可以访问实时 Web 搜索能力，获取当前市场情报。

## 📁 项目结构

```text
9_1_sequential_agent/
├── agent.py              # 具备搜索能力的业务实施计划生成器
├── app.py                # 用于业务规划的 Streamlit Web 界面
├── requirements.txt      # Python 依赖
└── README.md             # 本文档
```

## 🚀 快速开始

### 1. 安装依赖
```bash
cd 9_1_sequential_agent
pip install -r requirements.txt
```

### 2. 配置环境
创建 `.env` 文件并填入 Google API Key：
```bash
echo "GOOGLE_API_KEY=your_ai_studio_key_here" > .env
```

**重要**：API Key 可从 [Google AI Studio](https://aistudio.google.com/) 获取。

### 3. 运行 Streamlit 应用
```bash
streamlit run app.py
```

这会启动**业务实施计划生成器 Agent** 的 Web 界面。

## 🧪 工作原理

### **业务实施计划生成流水线**

Agent 会通过一个四阶段顺序工作流处理业务机会：

1. **🔍 市场分析**：使用 Web 搜索获取当前市场信息并进行竞品研究
2. **📊 SWOT 分析**：评估优势、劣势、机会与威胁
3. **🎯 战略制定**：形成战略目标与行动计划
4. **📋 实施规划**：生成详细执行路线图和资源需求

**核心亮点**：市场分析 Agent 可以调用一个专门的搜索 Agent。该搜索 Agent 被封装为 `AgentTool`，并通过 `google_search` 工具执行实时 Web 搜索。因此，顺序分析流水线使用的是当前市场情报，而不仅仅依赖模型训练数据。

`SequentialAgent` 会确保每一步都建立在上一步的输出之上，最终生成一份可执行的完整业务实施计划。

**最终结果**：包含市场调研、战略分析和执行路线图的完整业务实施方案。

## 🔧 本教程展示的 ADK 概念

### **1. SequentialAgent 模式**
作为核心工作流编排器，按顺序执行各个子 Agent，并确保后续步骤基于前一步输出继续处理。

### **2. AgentTool 集成**
将一个 Agent（搜索 Agent）封装为工具，再交由另一个 Agent（市场调研 Agent）调用，以扩展其能力。

### **3. Web 搜索能力**
通过集成搜索功能获取实时市场情报，使分析可以基于当前数据，而不局限于模型训练数据。

### **4. 子 Agent 专业化**
每个子 Agent 专注于业务分析流程中的一个阶段，从而形成模块化、易维护的系统。

### **5. Session 管理**
在整个分析流水线中维护会话状态，保证上下文能够在各个 Agent 之间传递。

### **6. Runner 执行**
通过 Runner 执行完整的业务实施工作流，并负责错误处理与响应管理。

## 🧪 可尝试的示例主题

- 城市中的**电动汽车充电站**
- **AI 驱动的医疗诊断**与患者护理
- **可持续食品配送**服务与包装
- **远程办公协作**工具和平台
- **可再生能源储能**解决方案

## 📊 预期输出

顺序 Agent 会生成：
1. **市场调研**：竞品分析与市场趋势
2. **SWOT 分析**：带有可执行洞察的战略评估
3. **战略计划**：明确的目标与实施步骤
4. **实施路线图**：可落地的执行指导

## 🎯 学习目标

- ✅ 理解 `SequentialAgent` 如何编排多个子 Agent
- ✅ 学会结合 Runner 与 Session 管理执行顺序 Agent
- ✅ 理解子 Agent 如何基于前一个 Agent 的输出继续处理
- ✅ 体验一个可直接运行的顺序工作流
- ✅ 理解如何通过 AgentTool 集成增强 Agent 能力

## 🚀 后续步骤

- 尝试不同业务主题，观察顺序工作流的表现
- 调整子 Agent 的执行顺序
- 向流水线中加入更多专业化 Agent
- 探索其他 ADK 工作流模式，例如并行和分支模式

## 🔧 故障排查

**常见问题：**
- **API Key 错误**：确认 `.env` 中已设置 `GOOGLE_API_KEY`
- **导入错误**：确认当前所在目录正确
- **搜索工具错误**：确认 API Key 具备使用搜索能力所需权限

**实用建议：**
- 先从简单主题开始，以便理解整体流程
- 使用 Streamlit 应用进行测试和可视化更方便
- 顺序模式非常适合确定性的分步骤流程
- Web 搜索集成能够提供实时市场情报

## 📚 关键要点

- **SequentialAgent** 非常适合必须按固定顺序执行的工作流
- **AgentTool 集成**允许不同 Agent 相互增强能力
- **Web 搜索能力**可以提供当前市场情报
- **子 Agent**既可以是简单的 `LlmAgent`，也可以是具备工具能力的复杂 Agent
- **清晰、可读的代码**有助于理解和修改
- **Streamlit 界面**为复杂 Agent 工作流提供了易用的交互入口
