# 🧠 DevPulseAI — 多 Agent 技术信号情报系统

这是一个参考实现，用于展示如何构建一条**多 Agent 流水线**：从多个技术来源聚合信号，对其相关性进行评分、评估潜在风险，并最终生成一份可执行的技术情报摘要。

> **设计理念：** 只有真正需要推理的环节才使用 Agent。数据采集、标准化、去重等确定性操作由普通工具函数完成，而不是为了“Agent 化”而强行包装成 Agent。

---

## 架构

```text
┌─────────────────────────────────────────────────────────┐
│                     数据来源                            │
│  GitHub · ArXiv · HackerNews · Medium · HuggingFace     │
└──────────────────────┬──────────────────────────────────┘
                       │ 原始信号
                       ▼
┌──────────────────────────────────────────────────────────┐
│  SignalCollector（工具 — 不使用 LLM）                   │
│  • 标准化为统一 Schema                                   │
│  • 使用 source:id 复合键去重                            │
│  • 过滤不完整信号                                        │
└──────────────────────┬───────────────────────────────────┘
                       │ 标准化后的信号
                       ▼
┌──────────────────────────────────────────────────────────┐
│  RelevanceAgent（Agent — gpt-4.1-mini）                 │
│  • 为每条信号计算 0–100 的开发者相关度                   │
│  • 综合考虑新颖性、影响、可执行性、时效性                │
│  • 未提供 API Key 时自动退回启发式规则                  │
└──────────────────────┬───────────────────────────────────┘
                       │ 已评分信号
                       ▼
┌──────────────────────────────────────────────────────────┐
│  RiskAgent（Agent — gpt-4.1-mini）                      │
│  • 分析安全漏洞                                          │
│  • 标记破坏性变更和弃用项                                │
│  • 风险等级：LOW / MEDIUM / HIGH / CRITICAL             │
└──────────────────────┬───────────────────────────────────┘
                       │ 已评估风险的信号
                       ▼
┌──────────────────────────────────────────────────────────┐
│  SynthesisAgent（Agent — gpt-4.1）                      │
│  • 交叉分析相关度和风险数据                              │
│  • 生成管理层摘要                                        │
│  • 输出可执行建议                                        │
└──────────────────────┬───────────────────────────────────┘
                       │
                       ▼
                 📄 技术情报摘要
```

---

## 为什么 Signal Collection 不是 Agent

这是一个**有意为之的架构选择**，而不是简化实现。

信号采集主要包括：

- 从 HTTP API 获取数据（确定性操作）
- 将字段标准化为统一 Schema（机械转换）
- 使用复合键去重（哈希比较）

**这些任务都不需要推理、判断或自然语言理解。**

如果把采集模块包装成 `Agent` 类，只会变成一种“装饰性 Agent”：虽然导入了 LLM，但实际上根本不会调用。这样反而会误导读者，以为这里必须使用大模型，而真实逻辑不过是 `for` 循环配合 `set()`。

> **经验法则：** 如果某段逻辑可以无歧义地写成纯函数，那么它应该是工具；如果输出依赖上下文理解、判断或自然语言生成，那么它才适合做成 Agent。

---

## Agent 职责与模型选择

| 组件 | 类型 | 模型 | 选择原因 |
|---|---|---|---|
| `SignalCollector` | **工具** | 无 | 确定性流程，不需要推理 |
| `RelevanceAgent` | **Agent** | `gpt-4.1-mini` | 分类任务，速度快、成本低，适合高吞吐 |
| `RiskAgent` | **Agent** | `gpt-4.1-mini` | 适合结构化分析，同时控制成本 |
| `SynthesisAgent` | **Agent** | `gpt-4.1` | 需要交叉分析和综合总结，使用更强推理模型 |

默认只使用**单一模型提供商 OpenAI**，以减少初始配置复杂度。也可以通过环境变量为各 Agent 单独覆盖模型：

```bash
export MODEL_RELEVANCE=gpt-4.1-nano    # 更便宜、更快
export MODEL_RISK=o4-mini               # 更适合深度风险推理
export MODEL_SYNTHESIS=gpt-4.1          # 默认，综合能力最强
```

---

## 如何运行

### 快速验证（无需 API Key）

```bash
cd advanced_ai_agents/multi_agent_apps/devpulse_ai
python verify.py
```

该命令使用 Mock 数据执行完整流水线，通常可在 **1 秒内**完成，不会发起网络请求，也不需要 API Key。

预期输出：

```text
[OK] DevPulseAI reference pipeline executed successfully
```

### 完整流水线（需要 API Key）

```bash
pip install -r requirements.txt
export OPENAI_API_KEY=sk-...
python main.py
```

如果没有配置 API Key，Agent 会自动退回启发式评分逻辑。

### Streamlit Dashboard

```bash
streamlit run streamlit_app.py
```

---

## 项目结构

```text
devpulse_ai/
├── agents/
│   ├── __init__.py              # 包导出与设计说明
│   ├── signal_collector.py      # 工具：标准化与去重
│   ├── relevance_agent.py       # Agent：相关性评分（gpt-4.1-mini）
│   ├── risk_agent.py            # Agent：风险评估（gpt-4.1-mini）
│   └── synthesis_agent.py       # Agent：生成情报摘要（gpt-4.1）
├── adapters/
│   ├── github.py                # GitHub 热门仓库
│   ├── arxiv.py                 # ArXiv 最新论文
│   ├── hackernews.py            # Hacker News 热门内容
│   ├── medium.py                # Medium AI/ML 博客
│   └── huggingface.py           # Hugging Face 热门模型
├── workflows/
│   └── signal-intelligence-pipeline.json
├── main.py                      # 完整流水线入口
├── verify.py                    # Mock 数据验证（<1 秒）
├── streamlit_app.py             # 交互式 Dashboard
└── requirements.txt             # 最小依赖，仅默认单一模型提供商
```

---

## 可选扩展（高级用户）

以下功能**不是参考实现的必要部分**，但可以用于进一步扩展架构：

1. **多模型提供商** — 将 `RelevanceAgent` 改为 Anthropic Claude 或 Google Gemini。`agno` 框架支持多个模型提供商。

2. **向量搜索** — 增加 Pinecone 或 Qdrant Adapter，长期保存技术信号并执行语义检索，用于趋势与模式识别。

3. **流式情报摘要** — 使用 `SynthesisAgent` 的 WebSocket Streaming 输出实时技术情报流。

4. **自定义 Adapter** — 实现新的 `fetch_*` 函数即可加入新的信号来源。函数应返回符合统一 Schema 的 `List[Dict]`，字段包括 `id`、`source`、`title`、`description`、`url`、`metadata`。

5. **反馈闭环** — 将用户反馈（👍/👎）存入 Supabase，并逐步用于优化相关性评分。

---

## 依赖

```text
agno              # Agent 框架
openai            # 默认 LLM Provider
httpx             # Adapter 使用的 HTTP 客户端
feedparser        # Medium RSS/Atom 解析
streamlit>=1.30   # 交互式 Dashboard
```

默认不需要安装 `google-generativeai`。如果希望扩展 Gemini 多模型支持，可单独安装新版 `google-genai`，不要使用已弃用的 `google-generativeai`。

---

## 架构取舍

| 决策 | 代价 | 原因 |
|---|---|---|
| 默认单一模型提供商 | 灵活性较低 | 初次配置只需要 1 个 API Key |
| 信号采集作为普通工具 | 看起来没那么“Agentic” | 架构更真实，只在需要推理时使用 Agent |
| 启发式回退机制 | 无 API Key 时质量较低 | 保证流水线即使在评估环境中也可以运行 |
| 每个来源默认 5 条信号 | 数据量较少 | 保持 Demo 足够快（API 模式 <10 秒，Mock <1 秒） |
| Agent 内不使用 Async | 吞吐量较低 | 代码更简单，更适合作为教学参考 |

---

_本项目作为 [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) 的参考实现构建。_
