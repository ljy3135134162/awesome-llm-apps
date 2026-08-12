# AG2 自适应研究团队

这是一个基于 Streamlit 的应用，结合了多 Agent 协作、Agent 路由决策以及失败回退机制，整体完全基于 AG2 构建。

## 本示例展示的内容

- **多 Agent 协作**：明确的角色划分与顺序式任务交接
- **Agent 路由能力**：通过清晰的决策步骤，在本地文档与 Web 搜索之间进行选择与回退
- **AG2 优先实现**：不依赖 Microsoft AutoGen，通过 `ag2[openai]` 安装

## 功能特性

- 支持上传本地文档（PDF、TXT、MD）
- 根据本地文档对问题的覆盖程度进行路由判断
- 可选的 Web 回退搜索，使用 SearxNG
- 通过 Verifier Agent 检查证据是否充分
- 最终生成带引用来源的综合答案

## 如何运行

1. 安装依赖：

```bash
pip install -r requirements.txt
```

2. 启动应用：

```bash
streamlit run app.py
```

3. 在侧边栏填写 OpenAI API Key，然后输入问题即可。

## 工作原理

1. **Triage Agent** 判断问题应优先基于本地文档回答，还是需要使用 Web 搜索。
2. **Local/Web Research Agent** 收集相关证据。
3. **Verifier Agent** 检查证据的充分性和可靠性。
4. **Synthesizer Agent** 整合信息，并生成带引用来源的最终回答。

## 可选扩展（AG2 0.11）

- **AG-UI 协议集成**：提供更丰富的 UI 渲染能力
- **OpenTelemetry 链路追踪**：用于调试多 Agent 工作流

这些功能均为可选项，不影响本示例的基础运行。

## 说明

- 默认模型为 `gpt-5-nano`。你可以在执行查询前通过侧边栏切换其他模型。
- Web 回退搜索默认使用 SearxNG 公共实例 `https://searxng.site/search`，该实例可能存在访问频率限制。
