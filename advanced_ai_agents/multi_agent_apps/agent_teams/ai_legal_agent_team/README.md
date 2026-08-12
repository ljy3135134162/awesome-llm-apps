# 👨‍⚖️ AI 法律智能体团队

### 🎓 免费分步教程
**👉 [点击这里查看完整的分步教程](https://www.theunwindai.com/p/build-an-ai-legal-team-run-by-ai-agents)，通过详细的代码讲解、说明和最佳实践，从零开始构建本项目。**

这是一个基于 Streamlit 的应用，通过多个 AI 智能体模拟一支完整的法律服务团队，对法律文件进行分析并提供全面的法律洞察。每个智能体分别承担不同的法律专业角色，包括法律研究、合同分析和法律策略规划等，并通过协同工作给出系统性的法律分析和建议。

## 功能特性

- **专业化法律 AI 智能体团队**
  - **法律研究员（Legal Researcher）**：配备 DuckDuckGo 搜索工具，用于查找并引用相关法律案件和判例。能够提供带有来源的详细研究摘要，并引用上传文件中的具体章节。
  
  - **合同分析师（Contract Analyst）**：专注于全面的合同审查，识别关键条款、义务和潜在问题，并引用文档中的具体条款进行详细分析。
  
  - **法律策略师（Legal Strategist）**：专注于制定综合法律策略，在同时考虑风险与机会的基础上给出可执行建议。
  
  - **团队负责人（Team Lead）**：协调各团队成员之间的分析，确保回答完整、建议具有可靠来源，并准确引用文档中的具体内容。其作用是协调上述三个智能体组成的 Agent Team。

- **文档分析类型**
  - 合同审查：由合同分析师负责
  - 法律研究：由法律研究员负责
  - 风险评估：由法律策略师、合同分析师负责
  - 合规检查：由法律策略师、法律研究员、合同分析师共同负责
  - 自定义问题：由整个智能体团队共同处理，包括法律研究员、法律策略师和合同分析师

## 运行方法

1. **配置环境**
   ```bash
   # 克隆仓库
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_legal_agent_team
   
   # 安装依赖
   pip install -r requirements.txt
   ```

2. **配置 API 密钥**
   - 从 [OpenAI Platform](https://platform.openai.com) 获取 OpenAI API 密钥
   - 从 [Qdrant Cloud](https://cloud.qdrant.io) 获取 Qdrant API 密钥和 URL

3. **运行应用**
   ```bash
   streamlit run legal_agent_team.py
   ```

4. **使用界面**
   - 输入 API 凭据
   - 上传法律文档（PDF）
   - 选择分析类型
   - 如有需要，添加自定义问题
   - 查看分析结果

## 注意事项

- 仅支持 PDF 文档
- 使用 GPT-4o 进行分析
- 使用 `text-embedding-3-small` 生成 Embedding
- 需要稳定的网络连接
- API 调用会产生相应费用
