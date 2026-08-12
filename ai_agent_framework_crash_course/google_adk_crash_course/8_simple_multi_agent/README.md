# 🎯 教程 8：简单多 Agent 研究助手（使用 ADK 运行）

## 🎯 你将学到什么
- 使用协调 Agent 与多个专业子 Agent 进行**多 Agent 编排**
- 构建多个 Agent 基于前序结果继续处理的**顺序工作流**
- 集成**Web 搜索**，获得实时研究能力
- 使用 **ADK Web** 交互式测试多 Agent 系统

## 🧠 核心概念：多 Agent 研究流水线
一个协调器 `LlmAgent` 按顺序编排三个专业 Agent：研究 → 总结 → 批判。每个 Agent 都会基于前一步结果继续加工，最终形成完整研究报告。

```text
用户问题 → 协调 Agent
                │
                ├──▶ 研究 Agent（Web 搜索 + 分析）
                │           │
                │           └──▶ 研究发现
                │
                ├──▶ 总结 Agent（综合整理）
                │           │
                │           └──▶ 关键洞察
                │
                └──▶ 批判 Agent（质量分析）
                            │
                            └──▶ 带建议的最终报告
```

## 📁 项目结构
```text
8_simple_multi_agent/
├── README.md                    # 本文件
├── requirements.txt             # 依赖
├── multi_agent_researcher/      # 主实现
│   ├── agent.py                 # 多 Agent 系统（导出 root_agent）
└── .env                         # 环境变量文件（需自行创建）
```

## 🚀 快速开始

### 1. 安装依赖
进入 `8_simple_multi_agent` 目录并安装所需库：
```bash
cd 8_simple_multi_agent
pip install -r requirements.txt
```

### 2. 配置环境
在 `8_simple_multi_agent` 目录中创建 `.env`：
```bash
# 创建 .env 文件
echo "GOOGLE_API_KEY=your_ai_studio_key_here" > .env
```

**重要**：请将 `your_ai_studio_key_here` 替换为你从 [Google AI Studio](https://aistudio.google.com/) 获取的实际 API Key。

### 3. 使用 ADK Web 运行（推荐）
在 `8_simple_multi_agent` 目录中执行：
```bash
adk web
```

**ADK Web 配置：**
- 打开终端输出的本地 URL
- 在导入区域使用以下路径：
  ```text
  ai_agent_framework_crash_course.google_adk_crash_course.8_simple_multi_agent.multi_agent_researcher
  ```
- 选择 `root_agent` 对象
- 开始与多 Agent 研究助手交互

## 🧪 可尝试的示例 Prompt

### **综合研究问题：**
```text
研究可再生能源在智慧城市中的未来整合方式，包括当前技术、实施挑战、经济可行性和政策要求，并给出批判性分析与建议。
```

### **其他测试问题：**
```text
研究欧盟当前 AI 监管现状及其对企业创新的影响。
```

```text
调查 CRISPR 基因编辑技术的最新进展，以及其在医学领域中的潜在应用。
```

```text
研究 K-12 教育中个性化学习平台的有效性，包括当前实施情况和学习成果。
```

## 🔍 工作原理

### **研究 Agent：**
- 使用 Google Search 进行全面 Web 研究
- 收集当前信息、趋势与最新进展
- 输出带来源和结构化提纲的研究结果

### **总结 Agent：**
- 将研究结果提炼为清晰、可执行的洞察
- 生成执行摘要和关键要点
- 识别重要模式和结论

### **批判 Agent：**
- 执行质量分析和缺口识别
- 提供风险与机会分析
- 给出可执行建议和后续步骤

### **协调器：**
- 编排整个研究工作流
- 确保严格按照“研究 → 总结 → 批判”的顺序执行
- 将所有结果整合成连贯的最终报告

## 📝 获得更好结果的建议
- 研究问题越**具体**，Agent 之间的协作效果越好
- 尽量让完整流程执行结束，以获得更全面的结果
- 系统会自动遵循研究流水线完成深入分析
- 每个 Agent 都会基于前一个 Agent 的输出继续加工

## 🔗 后续步骤
掌握本教程后，可以继续探索：
- **教程 9**：工作流 Agent（顺序、并行、分支）
- **高级模式**：自定义工具和 Agent 间通信
- **集成**：连接外部数据源和 API

## 🚨 故障排查
- **API Key 问题**：确认 `.env` 位于正确目录，且包含有效的 `GOOGLE_API_KEY`
- **导入错误**：确认使用上文所示的完整导入路径
- **找不到 Agent**：确认模块已正确导出 `root_agent`
