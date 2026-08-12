# 🤝 基于 Google ADK 的 AI 顾问 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-consultant-agent-with-gemini-2-5-flash)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 Google Agent Development Kit 构建的 AI 商业顾问，可结合实时 Web 研究提供全面的市场分析、战略规划以及可执行的商业建议。

## 功能特性

- **实时 Web 研究**：使用 Perplexity AI 搜索获取最新市场数据、趋势和竞争情报
- **市场分析**：结合 Web 搜索与 AI 洞察分析市场环境和潜在机会
- **战略建议**：生成包含时间表和实施计划的可执行商业策略
- **风险评估**：识别潜在风险并提供缓解策略
- **交互式 UI**：使用简洁的 Google ADK Web 界面进行咨询
- **评测系统**：内置评测、调试和 Session 跟踪能力

## 工作原理

1. **输入阶段**：用户通过 ADK Web 界面提交商业问题或咨询请求。
2. **研究阶段**：Agent 使用 Perplexity AI 开展实时 Web 研究，收集当前市场数据。
3. **分析阶段**：Agent 使用市场分析工具处理问题并生成洞察。
4. **策略阶段**：基于分析结果和 Web 研究生成战略建议。
5. **综合阶段**：将所有发现整合为一份带引用的完整咨询报告。
6. **输出阶段**：给出包含时间表和实施步骤的可执行建议。

## 环境要求

- Python 3.8+
- Google API Key（用于 Gemini 模型）
- Perplexity API Key（用于实时 Web 搜索）
- 所需 Python 依赖，详见 `requirements.txt`

## 安装

1. 克隆仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps
```

2. 安装依赖：

```bash
pip install -r requirements.txt
```

## 使用方式

1. 设置 API Keys：

```bash
export GOOGLE_API_KEY=your-google-api-key
export PERPLEXITY_API_KEY=your-perplexity-api-key
```

2. 启动 Google ADK Web 界面：

```bash
adk web
```

3. 在浏览器中打开 `http://localhost:8000`。

4. 从可用 Agent 列表中选择 `AI Business Consultant`。

5. 输入你的商业问题或咨询请求。

6. 查看包含实时 Web 数据和来源引用的完整分析与战略建议。

7. 使用 Eval 标签页保存并评估咨询 Session。

## 示例咨询主题

- “我想为小型企业推出一个 SaaS 创业项目。”
- “我是否应该把零售业务扩展到电商？”
- “医疗科技领域目前有哪些市场机会？”
- “我的新 FinTech 产品应该如何定位？”
- “进入可再生能源市场有哪些风险？”

## 技术细节

应用使用以下专用分析工具：

1. **Perplexity Search Tool**：使用 Perplexity AI 的 `sonar` 模型进行实时 Web 研究，获取最新市场数据、竞争对手信息和行业趋势，并返回来源引用。

2. **Market Analysis Tool**：处理商业问题，生成市场洞察、竞争分析以及机会识别结果。

3. **Strategic Recommendations Tool**：生成带优先级、时间表和实施路线图的可执行商业策略。

该 Agent 基于 Google ADK 的 `LlmAgent` 框架，使用 Gemini 2.5 Flash 模型，在实时 Web 研究支持下提供快速且高质量的商业咨询能力。

## 评测与测试

Agent 内置以下评测能力：

- **Session 管理**：跟踪咨询历史和执行进度
- **测试用例创建**：将效果良好的咨询 Session 保存为评测案例
- **性能指标**：监控工具调用情况和响应质量
- **自定义评测**：可针对特定业务需求配置评测指标
