# 并行执行

本示例展示如何使用 `asyncio.gather()` 同时运行多个 Agent，通过并发提升整体执行效率，并利用多个候选结果之间的差异提高最终输出质量。

## 🎯 本示例展示的内容

- **并行 Agent 执行**：同时运行多个 Agent
- **质量选择**：从多次候选结果中挑选最佳答案
- **翻译编排**：并行尝试多种语言表达方式
- **内容生成多样性**：让不同写作风格并行生成候选内容

## 🚀 快速开始

1. **安装 OpenAI Agents SDK**：
   ```bash
   pip install openai-agents
   ```

2. **配置环境**：
   ```bash
   cp ../env.example .env
   # 编辑 .env 并添加 OpenAI API Key
   ```

3. **运行 Agent**：
   ```python
   import asyncio
   from agent import main

   # 测试并行执行模式
   asyncio.run(main())
   ```

## 💡 核心概念

- **`asyncio.gather()`**：并发执行多个 Agent 调用
- **ItemHelpers**：从运行结果中提取文本输出
- **质量评估**：使用 Picker Agent 从多个候选结果中选择最佳答案
- **Trace 分组**：将同一个并行工作流组织到统一 Trace 中

## 🧪 可用示例

### 并行翻译 + 质量选择

```python
with trace("Parallel translation"):
    res_1, res_2, res_3 = await asyncio.gather(
        Runner.run(spanish_agent, msg),
        Runner.run(spanish_agent, msg),
        Runner.run(spanish_agent, msg),
    )

    best_translation = await Runner.run(
        translation_picker,
        f"Input: {msg}\n\nTranslations:\n{translations}"
    )
```

这里会让同一个翻译 Agent 并行生成多个候选结果，再由另一个 Agent 对结果进行比较和选择。

### 多风格翻译

- **正式西班牙语**：偏向使用 `usted` 等正式表达
- **日常西班牙语**：偏向使用 `tú` 等口语表达
- **地区化西班牙语**：例如采用墨西哥地区常见表达

### 内容生成多样性

- **创意写作**：强调画面感和叙事性
- **信息型写作**：强调清晰、准确和事实表达
- **说服型写作**：强调行动导向和说服力

## 💻 并行模式

### 通过重复执行提高质量

- 让同一个 Agent 多次生成结果
- 比较不同结果的一致性和完整度
- 从中选出质量最高的输出

### 通过专业化提高质量

- 为不同思路配置不同 Agent
- 让 Agent 分别负责不同专业方向
- 最后汇总多种视角

### 性能优化

- 并发执行通常比串行执行更快
- 多个独立任务可以同时等待模型响应
- 能更充分利用异步 I/O 和网络等待时间

## 🧠 什么时候适合并行 Agent

并行执行适合彼此**没有前后依赖关系**的任务，例如：

```text
                 ┌─ Agent A ─┐
用户请求 ────────┼─ Agent B ─┼──► 结果选择 / 汇总 ─► 最终输出
                 └─ Agent C ─┘
```

典型场景包括：
- 同一个问题生成多个候选答案
- 多个专家 Agent 分别分析不同维度
- 多语言或多风格内容生成
- 多个独立数据源同时处理

如果后一个 Agent 必须依赖前一个 Agent 的结果，则更适合使用串行编排，而不是并行执行。

## 🔗 后续步骤

- [Agents as Tools](../9_2_agents_as_tools/README.md) —— 学习 Agent 编排模式
- [教程 10：Tracing 与可观测性](../../10_tracing_observability/README.md) —— 监控和分析复杂工作流
