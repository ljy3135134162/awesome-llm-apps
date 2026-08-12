# 🧬 自进化 AI Agent

这是一个基于 [EvoAgentX](https://github.com/EvoAgentX/EvoAgentX) 构建的多 Agent 应用，可以把一个自然语言目标转化为可运行程序。它会**自动生成多 Agent 工作流**、执行工作流生成代码，并使用第二个模型对代码进行**验证和修复**，无需手动配置 Agent 之间的协作关系。

项目内置示例的目标是：*“生成一个可以直接在浏览器中玩的俄罗斯方块游戏 HTML 代码”*，最终会输出一个可直接运行的 `index.html`。

## ✨ 项目展示了什么

- **自动生成工作流**：`WorkFlowGenerator` 会根据自然语言目标自动设计 Agent 和执行步骤。
- **多 Agent 协作执行**：通过 `AgentManager` + `WorkFlow` 创建并运行自动生成的 Agent。
- **跨模型代码验证**：代码生成使用 OpenAI `gpt-4o-mini`，随后由独立的 Anthropic Claude 阶段负责检查并修复结果。
- **自进化式设计**：工作流由系统自动构建和优化，而不是由开发者手工写死。

## 🛠️ 如何开始

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_ai_agents/multi_agent_apps/ai_self_evolving_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
pip install git+https://github.com/EvoAgentX/EvoAgentX.git
```

3. **配置 API Keys**

```bash
export OPENAI_API_KEY=<your-openai-api-key>
export ANTHROPIC_API_KEY=<your-anthropic-api-key>
```

也可以把这些配置写入当前目录下的 `.env` 文件。

4. **运行 Agent**

```bash
python ai_Self-Evolving_agent.py
```

生成的游戏会保存到：

```text
examples/output/tetris_game/index.html
```

使用浏览器打开即可运行。

## 🔧 工作原理

1. **定义目标**：使用自然语言描述任务，例如“制作一个俄罗斯方块游戏”。
2. **生成工作流**：`WorkFlowGenerator` 根据目标自动生成多 Agent 执行图。
3. **执行工作流**：`AgentManager` 创建 Agent，`WorkFlow` 使用 `gpt-4o-mini` 执行整个流程。
4. **验证输出**：通过 LiteLLM 调用 Claude，由 `CodeVerification` 检查并修复生成代码。
5. **保存结果**：提取最终代码并写入输出目录。

## 📚 了解更多

本示例由开源框架 **EvoAgentX** 驱动。有关文档、教程以及 TextGrad、AFlow、MIPRO 等优化器，请查看 [EvoAgentX 仓库](https://github.com/EvoAgentX/EvoAgentX)。
