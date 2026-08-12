# 多模态 AI 设计智能体团队

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-multimodal-ai-agent-design-team)，通过详细的代码讲解、说明和最佳实践，从零开始构建本项目。**

这是一个基于 Streamlit 的应用，通过由 Google Gemini 模型驱动的专业 AI 智能体团队，提供全面的设计分析能力。

该应用利用多个专业 AI 智能体，对你的产品以及竞争对手的 UI/UX 设计进行综合分析，将视觉理解、用户体验评估与市场研究洞察结合起来。

## 功能特性

- **专业设计 AI 智能体团队**

   - 🎨 **视觉设计智能体**：评估设计元素、设计模式、配色方案、字体排版和视觉层级
   - 🔄 **UX 分析智能体**：评估用户流程、交互模式、易用性和无障碍性
   - 📊 **市场分析智能体**：提供市场洞察、竞争对手分析和产品定位建议

- **多种分析类型**：可选择视觉设计、UX 和市场分析
- **对比分析**：上传竞争对手设计，获得对比洞察
- **可自定义关注领域**：选择具体方面进行深入分析
- **上下文感知**：可补充额外背景信息，让分析结果更具针对性
- **实时处理**：通过进度指示快速获得分析结果
- **结构化输出**：获得组织清晰、可执行的分析洞察

## 运行方法

1. **配置环境**
   ```bash
   # 克隆仓库
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/multimodal_design_agent_team

   # 创建并激活虚拟环境（可选）
   python -m venv venv
   source venv/bin/activate  # Windows：venv\Scripts\activate

   # 安装依赖
   pip install -r requirements.txt
   ```

2. **获取 API Key**
   - 访问 [Google AI Studio](https://aistudio.google.com/apikey)
   - 创建 API 密钥

3. **运行应用**
   ```bash
   streamlit run design_agent_team.py
   ```

4. **使用应用**
   - 在侧边栏中输入 Gemini API 密钥
   - 上传设计文件（支持 JPG、JPEG、PNG）
   - 选择分析类型和关注领域
   - 如有需要，可添加额外上下文
   - 点击“Run Analysis（运行分析）”获取结果

## 技术栈

- **前端**：Streamlit
- **AI 模型**：Google Gemini 2.0
- **图像处理**：Pillow
- **市场研究**：DuckDuckGo Search API
- **框架**：Phidata，用于智能体编排

## 获得最佳效果的建议

- 上传清晰的高分辨率图片
- 尽量提供多个页面或界面截图，以补充上下文
- 加入竞争对手设计以进行对比分析
- 提供明确的目标用户背景信息

