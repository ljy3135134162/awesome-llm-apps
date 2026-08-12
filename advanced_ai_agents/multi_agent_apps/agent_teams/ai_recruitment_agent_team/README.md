# 💼 AI 招聘智能体团队

这是一个基于 Streamlit 的应用，通过多个 AI 智能体模拟完整的招聘团队，以自动化并简化招聘流程。每个智能体对应不同的招聘专业角色——从简历分析、候选人评估，到面试安排和沟通——协同工作以提供完整的招聘解决方案。系统将技术招聘、HR 协调和日程安排等能力整合为统一的自动化工作流。

## 功能特性

#### 专业化 AI 智能体

- 技术招聘智能体：分析简历并评估技术能力
- 沟通智能体：负责专业邮件往来
- 面试协调智能体：负责面试安排与协调
- 每个智能体都有明确的专业职责，并通过协作完成完整招聘流程

#### 端到端招聘流程
- 自动化简历筛选与分析
- 针对具体岗位的技术评估
- 专业候选人沟通
- 自动安排面试
- 集成式反馈系统

## 运行应用前的重要准备

- 为招聘人员创建或使用一个新的 Gmail 账号
- 为该 Gmail 账号启用两步验证，并生成应用专用密码（App Password）
- App Password 是一个 16 位代码（使用时不要包含空格），可通过 [Google App Password](https://support.google.com/accounts/answer/185833?hl=en) 生成。请按照页面步骤完成设置。密码形式类似 `afec wejf awoj fwrv`，在 Streamlit 应用中输入时请去掉空格
- 创建或使用一个 Zoom 账号，并前往 Zoom App Marketplace 获取 API 凭据：
[Zoom Marketplace](https://marketplace.zoom.us)
- 进入 Developer Dashboard，新建应用，选择 Server-to-Server OAuth，并获取 3 个凭据：Client ID、Client Secret 和 Account ID
- 随后需要为应用添加若干 Scope，以便创建候选人的 Zoom 面试链接并通过邮件发送
- 所需 Scope 包括：`meeting:write:invite_links:admin`、`meeting:write:meeting:admin`、`meeting:write:meeting:master`、`meeting:write:invite_links:master`、`meeting:write:open_app:admin`、`user:read:email:admin`、`user:read:list_users:admin`、`billing:read:user_entitlement:admin`、`dashboard:read:list_meeting_participants:admin`，其中最后 3 项为可选

## 运行方法

1. **配置环境**
   ```bash
   # 克隆仓库
   git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
   cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_recruitment_agent_team

   # 安装依赖
   pip install -r requirements.txt
   ```

2. **配置 API 凭据**
   - 用于访问 GPT-4o 的 OpenAI API Key
   - Zoom API 凭据（Account ID、Client ID、Client Secret）
   - 招聘人员邮箱的 Email App Password

3. **运行应用**
   ```bash
   streamlit run ai_recruitment_agent_team.py
   ```

## 系统组件

- **简历分析智能体**
  - 技能匹配算法
  - 工作经验核验
  - 技术能力评估
  - 候选人筛选决策

- **邮件沟通智能体**
  - 专业邮件撰写
  - 自动通知
  - 反馈沟通
  - 后续跟进管理

- **面试安排智能体**
  - Zoom 会议协调
  - 日历管理
  - 时区处理
  - 提醒系统

- **候选人体验**
  - 简单直观的上传界面
  - 实时反馈
  - 清晰沟通
  - 简化的招聘流程

## 技术栈

- **框架**：Phidata
- **模型**：OpenAI GPT-4o
- **集成**：Zoom API、Phidata 的 EmailTools 工具
- **PDF 处理**：PyPDF2
- **时间管理**：pytz
- **状态管理**：Streamlit Session State

## 免责声明

本工具旨在辅助招聘流程，但不应完全替代人类在招聘决策中的判断。所有自动化决策都应由人工招聘人员审核后再做最终确认。

## 未来增强方向

- 与 ATS 招聘管理系统集成
- 更高级的候选人评分机制
- 视频面试能力
- 技能测评集成
- 多语言支持
