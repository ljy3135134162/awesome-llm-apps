# 🎯 教程 2：模型无关 Agent

学习如何通过 OpenRouter 创建可使用**不同 AI 模型**的 Agent。本示例展示 ADK 如何通过独立的 Agent 实现使用 OpenAI 和 Anthropic 模型。

## 🎯 你将学到什么

- **OpenRouter 集成**：使用一个 API Key 访问多个模型提供商
- **独立 Agent 实现**：并排比较不同模型
- **工具集成**：为 Agent 添加简单工具
- **Root Agent 模式**：正确使用 ADK Agent 命名约定

## 🧠 核心概念：一个 API，多种模型

[OpenRouter](https://openrouter.ai/) 提供统一 API 来访问多个 AI 模型：
- ✅ **单一 API Key**：使用一个 Key 访问 OpenAI 和 Anthropic
- ✅ **便于比较**：运行不同 Agent 比较响应
- ✅ **成本灵活**：按使用量付费
- ✅ **避免供应商锁定**：可随时切换模型提供商

## 📁 项目结构

```text
2_model_agnostic_agent/
├── README.md                       # 本概览
├── requirements.txt                # 共享依赖
├── 2_1_openai_adk_agent/           # OpenAI GPT-4 Agent
│   └── agent.py                    # Agent 实现
└── 2_2_anthropic_adk_agent/        # Anthropic Claude Agent
    └── agent.py                    # Agent 实现
```

## 🔧 可用 Agent

### **OpenAI Agent**（`2_1_openai_adk_agent/`）
- **模型**：通过 OpenRouter 使用 GPT-4
- **Agent 名称**：`root_agent`（ADK 要求）
- **功能**：提供趣味事实工具，并采用 OpenAI 风格的响应方式

### **Anthropic Agent**（`2_2_anthropic_adk_agent/`）
- **模型**：通过 OpenRouter 使用 Claude 4 Sonnet
- **Agent 名称**：`root_agent`（ADK 要求）
- **功能**：提供趣味事实工具，并采用 Claude 风格的响应方式

## 🛠️ 配置与使用

### 1. **获取 OpenRouter API Key**
- 访问：[https://openrouter.ai/keys](https://openrouter.ai/keys)
- 注册并获取 API Key

### 2. **设置环境变量**
在每个 Agent 文件夹中创建 `.env` 文件：

**`2_1_openai_adk_agent/.env`：**
```bash
OPENROUTER_API_KEY=your_openrouter_api_key_here
```

**`2_2_anthropic_adk_agent/.env`：**
```bash
OPENROUTER_API_KEY=your_openrouter_api_key_here
```

### 3. **安装依赖**
```bash
# 在 2_model_agnostic_agent 目录中执行
pip install -r requirements.txt
```

### 4. **测试 OpenAI Agent**
```bash
adk web
```
然后在 ADK Web UI 中选择 `2_1_openai_adk_agent`。
- 尝试提问：`Tell me a fun fact!`
- 观察 OpenAI GPT-4 的响应风格

### 5. **测试 Anthropic Agent**
```bash
adk web
```
然后在 ADK Web UI 中选择 `2_2_anthropic_adk_agent`。
- 尝试提问：`Tell me a fun fact!`
- 与 GPT-4 的响应风格进行比较

## 💡 关键代码模式

每个 Agent 都遵循相同模式：

```python
from google.adk.agents import Agent
from google.adk.models.lite_llm import LiteLlm
import os

# 通过 OpenRouter 创建模型
model = LiteLlm(
    model="openrouter/openai/gpt-4",  # 或 Claude 模型
    api_key=os.getenv("OPENROUTER_API_KEY"),
    base_url="https://openrouter.ai/api/v1"
)

# 创建 root_agent（ADK 要求的名称）
root_agent = Agent(
    name="agent_name",
    model=model,
    instruction="Your instructions here...",
    tools=[your_tool_function],
)
```

## 🎯 学习目标

完成本教程后，你将理解：
- ✅ 如何将 OpenRouter 与 ADK 配合使用
- ✅ 如何为不同模型创建独立 Agent
- ✅ 如何比较不同 AI 提供商的响应
- ✅ 如何使用 `root_agent` 正确组织 ADK Agent

## 🔄 比较模型

1. **运行 OpenAI Agent**并提出问题
2. 使用相同问题**运行 Anthropic Agent**
3. **观察两者差异**，包括响应风格和处理方式
4. 使用不同类型的 Prompt **进行实验**

## 💰 成本信息

- OpenRouter 按 Token 使用量收费
- GPT-4o：成本较高，但能力较强
- Claude 4 Sonnet：成本与性能较均衡
- 可以在 OpenRouter Dashboard 中设置消费上限
- 提供可用于测试的免费层级

## 🚨 重要说明

- **Root Agent**：每个 Agent 都必须以 `root_agent` 变量暴露，ADK 才能识别
- **环境变量**：每个文件夹都需要自己的 `.env` 文件
- **API Key**：两个 Agent 可以使用同一个 OpenRouter Key
- **模型比较**：分别运行 Agent，以比较不同模型的行为
