### AI 邮件 GTM 外联 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-email-gtm-outreach-agent-team)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个端到端、多 Agent 的 Streamlit 应用，使用 GPT-5 和 Exa 自动化 B2B 外联。它可以自动发现相关公司、寻找合适的联系人（创始人办公室、GTM/销售负责人、合作伙伴/商务拓展、产品营销等），研究官网和 Reddit 信息，并按照你选择的风格生成个性化外联邮件。

## 功能特性

- **多 Agent 工作流**：
  - **Company Finder**：使用 Exa 查找符合目标条件和产品/服务定位的公司。
  - **Contact Finder**：为每家公司寻找 2–3 位相关决策者及其邮箱；如邮箱为推测所得，会明确标注。
  - **Researcher**：从公司官网和 Reddit 中提取 2–4 条有价值的信息，用于实现真正有针对性的个性化。
  - **Email Writer**：使用 GPT-5 生成简洁、结构清晰的外联邮件。

- **操作控制项**：
  - 可设置目标公司数量（1–10）
  - 可选择邮件风格：Professional、Casual、Cold 或 Consultative
  - 提供按阶段展示的实时进度界面，并使用清晰分区展示结果

- **安全优先**：
  - API Key 通过 Streamlit 侧边栏输入，不会硬编码或提交到仓库中

## 环境要求

通过 `requirements.txt` 安装依赖：

```bash
pip install -r advanced_ai_agents/multi_agent_apps/ai_email_gtm_outreach_agent/requirements.txt
```

需要以下环境变量，可通过侧边栏或系统 Shell 设置：

- `OPENAI_API_KEY`
- `EXA_API_KEY`

## 运行方式

```bash
streamlit run advanced_ai_agents/multi_agent_apps/ai_email_gtm_outreach_agent/ai_email_gtm_outreach_agent.py
```

## 使用方法

1. 在左侧边栏输入 `OPENAI_API_KEY` 和 `EXA_API_KEY`。
2. 填写目标客户描述以及你提供的产品或服务。
3. 选择目标公司数量和邮件风格。
4. 点击 `Start Outreach`，即可查看整个流程：Companies → Contacts → Research → Emails。
5. 查看公司、联系人、研究洞察，并下载或复制生成的邮件建议。

## 说明

- 应用通过 OpenAI 使用 `gpt-5` 模型。如果你的账号无法使用该模型，可在 `ai_email_gtm_outreach_agent.py` 中切换为已有权限的模型。
- Web 公司发现由 Exa 完成，请确保 `EXA_API_KEY` 有效。

## 故障排查

- 如果某个阶段长时间没有继续，请检查 API Key 和网络连接。
- 如果出现 JSON 解析错误，可重新运行该阶段；模型有时会在 JSON 前后附加额外文本。
- 如果触发速率限制，可减少目标公司数量。
