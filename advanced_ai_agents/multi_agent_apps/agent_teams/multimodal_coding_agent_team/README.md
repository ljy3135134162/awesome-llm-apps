# 💻 基于 o3-mini 与 Gemini 的多模态 AI 编程智能体团队

这是一个由 AI 驱动的 Streamlit 应用，可作为你的个人编程助手。应用由多个基于新 o3-mini 模型构建的智能体协同工作。你可以上传编程题目的图片，也可以直接用文字描述问题；AI 智能体会进行分析、生成最优解，并在沙箱环境中执行代码。

## 功能特性

#### 多模态问题输入
- 上传编程题目图片（支持 PNG、JPG、JPEG）
- 使用自然语言输入问题
- 自动从图片中提取题目内容
- 交互式问题处理

#### 智能代码生成
- 生成具有最佳时间复杂度和空间复杂度的优化方案
- 输出整洁、带文档说明的 Python 代码
- 支持类型提示和规范文档
- 处理边界情况

#### 安全代码执行
- 在沙箱环境中执行代码
- 实时返回执行结果
- 错误处理与解释
- 30 秒执行超时保护

#### 多智能体架构
- 视觉智能体（Gemini-2.0-flash）：负责图像处理
- 编程智能体（OpenAI o3-mini）：负责生成解决方案
- 执行智能体（OpenAI）：负责运行代码并分析结果
- 使用 E2B Sandbox 进行安全代码执行

## 运行方法

按照以下步骤配置并运行应用：
- 获取 OpenAI API Key：https://platform.openai.com/
- 获取 Google（Gemini）API Key：https://makersuite.google.com/app/apikey
- 获取 E2B API Key：https://e2b.dev/docs/getting-started/api-key

1. **克隆仓库**
   ```bash
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/multimodal_coding_agent_team
   ```

2. **安装依赖**
   ```bash
   pip install -r requirements.txt
   ```

3. **运行 Streamlit 应用**
   ```bash
   streamlit run ai_coding_agent_o3.py
   ```

4. **配置 API Key**
   - 在侧边栏中输入 API Key
   - 若要使用完整功能，需要同时提供 OpenAI、Gemini 和 E2B 三个 API Key

## 使用方法

1. 上传编程题目的图片，或者直接输入问题描述
2. 点击“Generate & Execute Solution（生成并执行解决方案）”
3. 查看生成的完整解决方案及其文档说明
4. 查看执行结果以及生成的文件
5. 检查错误信息或执行超时提示
