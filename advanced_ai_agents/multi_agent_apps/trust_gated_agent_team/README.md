# 🛡️ 信任门控多 Agent 研究团队

构建一条多 Agent 研究流水线：每个 AI Agent 在参与任务前都必须通过**信任验证**，并且每一次操作都会记录到一个可独立验证的**哈希链审计轨迹**中。

## 功能特性

- **信任门控** —— Agent 会获得 0–100 的信任评分，并划分为 Gold / Silver / Bronze 等级。只有达到最低阈值的 Agent 才能参与任务
- **密码学审计轨迹** —— 每个 Agent 操作都会使用 SHA-256 哈希记录，并与上一条记录串联。任何一条记录被篡改，后续哈希都会失效
- **多 Agent 流水线** —— Researcher → Analyst → Writer，后一个 Agent 基于前一个 Agent 的输出继续工作
- **可视化 Dashboard** —— 可以直观看到哪些 Agent 通过验证、哪些被阻止，并验证完整审计链
- **极少外部依赖** —— 整体完全自包含，只需要 `openai` 和 `streamlit`

## 工作原理

```
                ┌─────────────────────┐
                │   Trust Registry    │
                │   （验证 Agent）     │
                └──┬───────┬───────┬──┘
                   │       │       │
             ┌─────▼──┐ ┌──▼────┐ ┌▼────────┐
             │Research │ │Analyst│ │ Writer  │
             │ ✅ 75   │ │ ✅ 60 │ │ 🚫 5   │
             └────┬───┘ └──┬────┘ └─────────┘
                  │        │
                  ▼        ▼
          ┌──────────────────────┐
          │  Research Pipeline   │
          │   （仅可信 Agent）    │
          └──────────┬───────────┘
                     │
                     ▼
          ┌──────────────────────┐
          │  Hash-Chained Audit  │
          │   （防篡改审计链）     │
          └──────────────────────┘
```

1. **Trust Check** —— 检查每个 Agent 的信任评分是否达到最低阈值
2. **Gate** —— 低于阈值的 Agent 会被阻止进入流水线
3. **Execute** —— 通过验证的 Agent 按顺序执行，并逐步基于前序结果继续工作
4. **Audit** —— 所有操作，包括信任检查本身，都会写入哈希链

## 开始使用

### 环境要求

- Python 3.9+
- OpenAI API Key

### 安装

```bash
pip install -r requirements.txt
```

### 设置 API Key（可选，也可以直接在侧边栏粘贴）

```bash
export OPENAI_API_KEY=your-api-key
```

### 运行

```bash
streamlit run trust_gated_agents.py
```

### 快速体验（3 步）

1. 在侧边栏粘贴 OpenAI API Key
2. 点击 **Run Trust-Gated Pipeline** —— 默认会预先选择多个 Agent，其中 Writer 是一个不可信 Bot
3. 观察结果：Researcher（75 分）和 Analyst（60 分）通过，Untrusted Bot（5 分）被阻止

将 Writer 下拉框切换为 `Report Writer (score 45)`，即可看到三个 Agent 全部通过验证。

## 审计轨迹

审计轨迹采用与区块链交易日志类似的哈希链模式：

```json
[
  {
    "seq": 0,
    "agent": "researcher-001",
    "action": "trust_verification",
    "hash": "a1b2c3...",
    "prev_hash": "0000000000000000000000000000000000000000000000000000000000000000"
  },
  {
    "seq": 1,
    "agent": "researcher-001",
    "action": "pipeline_step_1",
    "hash": "d4e5f6...",
    "prev_hash": "a1b2c3..."
  }
]
```

每条记录的 `hash` 都由以下字段共同计算得到：`sequence + timestamp + agent + action + input_hash + output_hash + trust_score + prev_hash`。

只要修改任意一条记录中的任意字段，后续所有哈希都会失效。

导出的 JSON 可以被独立验证，不需要特殊工具，只需要 SHA-256。

## 为什么这很重要

在多 Agent 系统中，两个问题会互相放大：

1. **信任** —— 在把任务交给 Agent 之前，如何判断它是否可靠？
2. **问责与追踪** —— 如果出了问题，如何重建整个执行过程？

Trust Gating 通过在执行前验证 Agent 凭据和评分解决第一个问题；审计轨迹通过创建防篡改记录解决第二个问题。

这些审计记录保存在 Agent 自身执行上下文之外，而不是仅存在于 Agent 的记忆中，因此即使 Agent 本身出现错误，也仍然可以追踪整个过程。

## 技术栈

- **Streamlit** —— 提供交互式 UI 和可视化信任 Dashboard
- **OpenAI** —— 使用 GPT-4o-mini 进行 Agent 推理
- **SHA-256** —— 构建哈希链审计轨迹，不依赖额外密码学库
