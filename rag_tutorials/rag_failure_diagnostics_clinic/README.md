# RAG 故障诊断诊所

这是一个小型、与框架无关的 **RAG 故障诊断工具**。

你可以粘贴来自 LLM + RAG 流水线的真实 Bug 描述。  
脚本会让 LLM 将故障归类到若干个 **可复用的故障模式** 之一，并给出一个 **最小化的结构性修复方案**，而不是仅仅建议“增加更多上下文”或“换一个更好的模型”。

目标是展示一种基于模式的方法来调试 RAG 故障，并且这种方法可以适配任何技术栈：LangChain、LlamaIndex、自定义微服务或内部基础设施。

---

## 你将学到什么

通过运行这个示例，你将学习如何：

- 用自然语言描述 **真实世界中的 RAG Bug**，让 LLM 对其进行推理。
- 使用一个小型 **故障模式库** 快速分诊问题。
- 要求模型提出 **最小化的结构性改动**，而不是单纯调整 Prompt。
- 从一个小型 Python 脚本调用 **OpenAI 兼容 API**。
- 将每次诊断保存为 JSON 报告，便于后续分析或事故复盘。

这不是一个完整框架。  
它是一个紧凑的 **诊断工具示例**，展示了一种可以应用到你自己技术栈中的模式。

---

## 目录结构

本教程默认 `rag_tutorials/rag_failure_diagnostics_clinic` 目录中包含以下文件：

- `README.md` ← 当前文件  
- `rag_failure_diagnostics_clinic.py` ← 最小交互式 CLI 脚本  
- `requirements.txt` ← Python 依赖  

脚本完全自包含。  
所有故障模式定义和 Prompt 都位于此目录中。

---

## 故障模式（P01–P12）

该诊断工具内置了一套较为明确的 **12 种可复用故障模式**。
每个 Bug 会被映射到一个主要故障模式，同时可以附带次要候选模式。

你可以根据自己的生产事故修改或扩展这些模式。

| ID | 模式名称 | 典型症状 |
| --- | --- | --- |
| P01 | 检索幻觉 / Grounding 漂移 | 回答很自信，但与检索到的文档相矛盾。 |
| P02 | Chunk 边界或切分问题 | 相关事实被拆散或截断在不同 Chunk 中。 |
| P03 | Embedding 不匹配 / 语义与向量距离不一致 | 余弦相似度与真实相关性不一致。 |
| P04 | 索引偏斜或数据过期 | 即使真实数据源已更新，系统仍返回旧数据或缺失数据。 |
| P05 | 查询重写或 Router 路由偏差 | Router 将查询发送到错误的工具或数据集。 |
| P06 | 长链推理漂移 | 多步骤任务逐渐丢失之前的约束条件。 |
| P07 | 工具调用错误或缺少 Grounding | 工具使用了错误参数，或在缺乏依据的情况下调用。 |
| P08 | 会话记忆泄漏 / 上下文缺失 | 对话在不同轮次或不同会话之间丢失重要事实。 |
| P09 | 评估盲区 | 系统可以通过测试，但在真实事故中失败。 |
| P10 | 启动顺序问题 / 依赖尚未就绪 | 部署后的前几分钟服务崩溃或返回 5xx。 |
| P11 | 不同环境中的配置或 Secret 漂移 | 本地可以运行，但在 Staging / Production 中因配置问题失败。 |
| P12 | 多租户 / 多 Agent 相互干扰 | 不同请求或 Agent 互相覆盖状态或资源。 |

内置示例大致对应：

- 示例 1 → 检索幻觉 / Grounding 漂移（P01）。  
- 示例 2 → 启动顺序 / 依赖未就绪（P10）。  
- 示例 3 → 环境配置或 Secret 漂移（P11）。

建议你将这些示例替换为自己真实的事故片段。

---

## 工作原理

整体流程如下：

1. 脚本构建一个 **System Prompt**，向模型说明上述 12 种故障模式。
2. 你选择三个内置示例之一，或粘贴自己的 RAG / LLM Bug 描述。
3. 模型需要：
   - 选择一个 **主要模式 ID**（P01–P12）。  
   - 可选选择最多 **两个次要候选模式**。  
   - 用简短要点说明判断依据。  
   - 提出一个 **最小化结构性修复方案**，例如修改检索、路由、评估或基础设施。  
4. 完整回答会打印到控制台，并与原始 Bug 文本和模型名称一起保存到 `rag_failure_report.json`。

该示例旨在展示：如何通过一个小型 **故障模式词汇表 + Prompt**，将 LLM 变成一个轻量级事故分诊助手。

---

## 环境要求

- Python 3.9 或更高版本。
- 任意 **OpenAI 兼容** Chat Completion Endpoint 的 API Key：
  - 例如，使用 `OPENAI_API_KEY` 调用 `https://api.openai.com/v1`。
  - 或通过 `OPENAI_BASE_URL` 配置你自己的代理地址。
- 基本了解 RAG 流水线、日志和常见故障模式。

---

## 安装

从 `awesome-llm-apps` 仓库根目录进入：

```bash
cd rag_tutorials/rag_failure_diagnostics_clinic
pip install -r requirements.txt
```

最小化 `requirements.txt`：

```text
openai>=1.6.0
```

建议将 API Key 设置为环境变量：

```bash
export OPENAI_API_KEY="sk-..."
# 可选：如果你使用自定义 Endpoint
# export OPENAI_BASE_URL="https://your-proxy.example.com/v1"
# export OPENAI_MODEL="gpt-4o-mini"
```

> 提示：如果更喜欢 Colab，也可以直接将整个 `rag_failure_diagnostics_clinic.py` 文件复制到一个 Colab Cell 中运行。

---

## 运行诊断工具

在 `rag_tutorials/rag_failure_diagnostics_clinic` 目录中执行：

```bash
python rag_failure_diagnostics_clinic.py
```

你会看到一个简单的文本界面：

- 如果没有设置 `OPENAI_API_KEY`，脚本会要求你输入 API Key。
- 你可以继续使用默认 Base URL（`https://api.openai.com/v1`）和模型（`gpt-4o`），也可以覆盖它们。
- 然后可以选择：
  - `1` → 内置检索幻觉示例（P01）。
  - `2` → 启动顺序问题示例（P10）。
  - `3` → 配置 / Secret 漂移示例（P11）。
  - `p` → 粘贴你自己的 Bug 描述。

每次运行都会打印诊断结果，并写入一个 `rag_failure_report.json` 文件，其中包含 Bug 文本、模型配置以及助手回复。

你可以将多个报告提交到自己的仓库中，作为一个轻量级的 **RAG 事故案例库**。

---

## 扩展方向

可以进一步尝试：

- 使用你自己日志中的匿名事故替换内置示例。
- 添加更多故障模式，或根据你的技术栈将已有模式进一步细分。
- 输出更丰富的 JSON Schema，例如严重程度、负责人、疑似组件等字段。
- 将报告接入评估仪表盘或事故跟踪系统。
