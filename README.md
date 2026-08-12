<p align="center">
  <a href="http://www.theunwindai.com">
    <img src="docs/banner/unwind_black.png" width="900px" alt="Unwind AI">
  </a>
</p>

<div align="center">

# Awesome LLM Apps

**100+ 个开源 AI Agent、Agent Skill 与 RAG 应用。手工构建、端到端测试，并采用 Apache-2.0 许可证。**

克隆即可使用、部署甚至商业化 —— 100% 免费且开源。

支持 Claude、Gemini、GPT、DeepSeek、Llama、Qwen 以及其他开源模型。

**[Unwind AI 分步教程](https://www.theunwindai.com)** · **[快速开始](#-立即运行一个示例)** · **[浏览全部模板](#-浏览全部模板)**

> 🇨🇳 本仓库为 `awesome-llm-apps` 的中文本地化 Fork。项目代码、链接、许可证及原作者信息保持不变，主要对说明文档进行中文化，便于中文开发者阅读和使用。

<a href="https://trendshift.io/repositories/9876" target="_blank">
  <img src="https://trendshift.io/api/badge/repositories/9876" width="220" alt="Featured on Trendshift as the #1 repository of the day">
</a>

<br>

</div>

<table>
  <tr>
    <td width="33.3%" align="center">
      <a href="agent_skills/project-graveyard/"><img src="docs/gallery/project-graveyard.png" alt="Project Graveyard: 分析那些被你放弃的项目"></a>
      <sub><b>Project Graveyard</b></sub>
    </td>
    <td width="33.3%" align="center">
      <a href="voice_ai_agents/insurance_claim_live_agent_team/"><img src="docs/gallery/insurance-claim-live-team.png" alt="Insurance Claim Live Agent Team: 实时处理保险理赔"></a>
      <sub><b>Insurance Claim Live Agent Team</b></sub>
    </td>
    <td width="33.3%" align="center">
      <a href="advanced_ai_agents/single_agent_apps/ai_fraud_investigation_agent/"><img src="docs/gallery/ai-fraud-investigation.png" alt="AI Fraud Investigation Agent: 交叉核验公开记录"></a>
      <sub><b>AI Fraud Investigation Agent</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="agent_skills/self-improving-agent-skills/"><img src="docs/gallery/self-improving-agent-skills.png" alt="Self-Improving Agent Skills: 基于评测自动改进 Skill"></a>
      <sub><b>Self-Improving Agent Skills</b></sub>
    </td>
    <td align="center">
      <a href="advanced_ai_agents/multi_agent_apps/ai_home_renovation_agent"><img src="docs/gallery/ai-home-renovation.png" alt="AI Home Renovation Agent: 从照片生成逼真的装修设计"></a>
      <sub><b>AI Home Renovation Agent</b></sub>
    </td>
    <td align="center">
      <a href="always_on_agents/always_on_hn_briefing_agent/"><img src="docs/gallery/always-on-hn-briefing.png" alt="Always-on HN Briefing Agent: 持续阅读 Hacker News 并生成简报"></a>
      <sub><b>Always-on HN Briefing Agent</b></sub>
    </td>
  </tr>
</table>

## 🚀 立即运行一个示例

10 秒内为你的编程 Agent 添加一个新 Skill：

```bash
npx skills add https://github.com/Shubhamsaboo/awesome-llm-apps/tree/main/agent_skills/project-graveyard
```

然后问它：*“为什么我的业余项目总是做不完？”*

或者克隆仓库，并在约 30 秒内运行任意 Agent：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/ai_travel_agent
pip install -r requirements.txt
streamlit run travel_agent.py
```

> 📬 每周都会发布新模板。[可通过 Unwind AI 接收更新](https://www.theunwindai.com)。

## 📂 浏览全部模板

### 🧩 Agent Skills

*为你的编程 Agent 增加新能力。只需一条命令安装，然后直接使用自然语言调用。每个 Skill 都包含真实可运行代码，并通过安全检查与评测 CI。兼容 Claude Code、Codex、Cursor 及其他编程 Agent。[浏览全部 Skills →](agent_skills/)*

*   [⚰️ Project Graveyard](agent_skills/project-graveyard/) - 找出你放弃过的所有业余项目，分析它们为何停止，并帮助你重新完成最值得继续的那个
*   [🔭 Scope Creep Detector](agent_skills/scope-creep-detector/) - 检查代码 diff 是否超出原始目标，并建议哪些内容应保留、拆分或补充说明
*   [🏺 Commit Archaeologist](agent_skills/commit-archaeologist/) - 从最初引入代码的提交、后续修改、关联变更和意图线索中，重建某个文件或代码区域存在的原因
*   [🩺 Dependency Doctor](agent_skills/dependency-doctor/) - 检查依赖清单中的标准库误固定、过时 backport、未固定版本、重复约束及已撤回版本
*   [🧠 Advisor Orchestrator Worker](agent_skills/advisor-orchestrator-worker/) - 元循环架构：Claude Fable 5 作为顾问、GPT-5.6 作为编排器、Gemini 3.5 Flash 作为执行者
*   [♾️ Self-Improving Agent Skills](agent_skills/self-improving-agent-skills/) - 使用 Gemini 和 ADK 自动优化 Agent Skills

### 🌱 入门级 AI Agents

*单文件 Agent，只需 API Key 即可运行，非常适合作为起点。*

*   [🎙️ AI Blog to Podcast Agent](starter_ai_agents/ai_blog_to_podcast_agent/) - 将任意博客 URL 转换成带旁白的播客节目
*   [❤️‍🩹 AI Breakup Recovery Agent](starter_ai_agents/ai_breakup_recovery_agent/) - 通过 Agent 团队帮助用户度过分手后的情绪低谷
*   [📊 AI Data Analysis Agent](starter_ai_agents/ai_data_analysis_agent/) - 使用自然语言对任意 CSV 或 Excel 文件提问
*   [🩻 AI Medical Imaging Agent](starter_ai_agents/ai_medical_imaging_agent/) - 使用 Gemini 对 X 光片和扫描影像进行分析
*   [😂 AI Meme Generator Agent (Browser)](starter_ai_agents/ai_meme_generator_agent_browseruse/) - 直接驱动真实浏览器生成 Meme，而不是调用图像 API
*   [🎵 AI Music Generator Agent](starter_ai_agents/ai_music_generator_agent/) - 输入提示词，直接生成 MP3 音轨
*   [🛫 AI Travel Agent (Local & Cloud)](starter_ai_agents/ai_travel_agent/) - 按天生成个性化旅行行程
*   [✨ Gemini Multimodal Agent](starter_ai_agents/multimodal_ai_agent/) - 在一个 Agent 中结合视频分析与 Web 搜索
*   [🔄 Mixture of Agents](starter_ai_agents/mixture_of_agents/) - 多个 LLM 分别回答，再由一个模型聚合最佳结果
*   [📊 xAI Finance Agent](starter_ai_agents/xai_finance_agent/) - 基于 Grok 的实时股票分析
*   [🔍 OpenAI Research Agent](starter_ai_agents/openai_research_agent/) - 使用 OpenAI Agents SDK 进行多 Agent 主题研究
*   [🕸️ Web Scraping AI Agent](starter_ai_agents/web_scraping_ai_agent/) - 用自然语言描述要提取的内容，Agent 自动完成网页抓取

### 🚀 高级 AI Agents

*面向生产级场景设计的 Agent，包含工具调用、记忆能力和多步推理。*

*   [🏚️ 🍌 AI Home Renovation Agent with Nano Banana Pro](advanced_ai_agents/multi_agent_apps/ai_home_renovation_agent) - 输入房间照片，输出装修方案与逼真的效果图
*   [🧠 DevPulse AI - Multi-Agent Signal Intelligence](advanced_ai_agents/multi_agent_apps/devpulse_ai/) - 聚合并评分技术信号，生成每日情报摘要
*   [🔍 AI Deep Research Agent](advanced_ai_agents/single_agent_apps/ai_deep_research_agent/) - 使用 OpenAI Agents SDK 和 Firecrawl 进行全面 Web 研究
*   [📊 AI VC Due Diligence Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_vc_due_diligence_agent_team) - 基于 Gemini 3 的多 Agent 创业公司投资尽调
*   [🔬 AI Research Planner & Executor (Google Interactions API)](advanced_ai_agents/single_agent_apps/research_agent_gemini_interaction_api) - 多阶段研究、状态化对话，并自动生成信息图
*   [🤝 AI Consultant Agent](advanced_ai_agents/single_agent_apps/ai_consultant_agent) - 结合实时 Web 研究生成市场分析和战略建议
*   [🏗️ AI System Architect Agent](advanced_ai_agents/single_agent_apps/ai_system_architect_r1/) - 结合 DeepSeek R1 推理与 Claude 进行架构评审
*   [💰 AI Financial Coach Agent](advanced_ai_agents/multi_agent_apps/ai_financial_coach_agent/) - 个性化预算、债务和储蓄分析
*   [🎬 AI Movie Production Agent](advanced_ai_agents/single_agent_apps/ai_movie_production_agent/) - 根据一句电影创意生成剧本草稿与选角建议
*   [📈 AI Investment Agent](advanced_ai_agents/single_agent_apps/ai_investment_agent/) - 基于 Yahoo Finance 数据生成股票对比报告
*   [📡 Earnings Call Analyst Agent](advanced_ai_agents/single_agent_apps/earnings_call_analyst_agent/) - 将 YouTube 财报电话会议转换为与播放进度同步的分析工作区
*   [🏋️‍♂️ AI Health & Fitness Agent](advanced_ai_agents/single_agent_apps/ai_health_fitness_agent/) - 根据个人目标生成饮食和训练计划
*   [🚀 AI Product Launch Intelligence Agent](advanced_ai_agents/multi_agent_apps/product_launch_intelligence_agent) - 分析竞争对手发布活动并生成市场进入情报
*   [🔍 AI Fraud Investigation Agent](advanced_ai_agents/single_agent_apps/ai_fraud_investigation_agent/) - 交叉核对公开记录，识别存在异常的机构或设施
*   [🗞️ AI Journalist Agent](advanced_ai_agents/single_agent_apps/ai_journalist_agent/) - 自动研究、撰写并编辑任意主题文章
*   [🧠 AI Mental Wellbeing Agent](advanced_ai_agents/multi_agent_apps/ai_mental_wellbeing_agent/) - 多 Agent 协作生成心理健康支持计划
*   [📑 AI Meeting Agent](advanced_ai_agents/single_agent_apps/ai_meeting_agent/) - 在会议前准备背景资料、行业洞察和策略简报
*   [🧬 AI Self-Evolving Agent](advanced_ai_agents/multi_agent_apps/ai_self_evolving_agent/) - 基于 EvoAgentX 自动重写自身工作流的 Agent
*   [👨🏻‍💼 AI Sales Intelligence Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_sales_intelligence_agent_team) - 实时生成竞争销售 Battle Card
*   [🎧 AI Social Media News and Podcast Agent](advanced_ai_agents/multi_agent_apps/ai_news_and_podcast_agents/) - 将可信来源整理成资讯简报与自动生成的播客
*   [🌐 Openwork - Open Browser Automation Agent](https://github.com/accomplish-ai/openwork) <sub>↗ 外部项目</sub> - 可操作真实浏览器的开源 Agent
*   [🛡️ Trust-Gated Multi-Agent Research Team](advanced_ai_agents/multi_agent_apps/trust_gated_agent_team/) - 每个 Agent 都经过验证，每次操作都记录在哈希链审计轨迹中

### 🛰️ 常驻型 Agents

*在计划任务或事件触发下持续运行，监控变化中的上下文，判断哪些信息需要关注，并主动发送更新、产物或执行操作。*

*   [📰 Always-on Hacker News Briefing Agent](always_on_agents/always_on_hn_briefing_agent/) - 定时抓取 Hacker News，并将排序后的每日简报发送到 Slack 或邮箱
*   [📡 Release Radar Agent](always_on_agents/release_radar_agent/) - 监控依赖版本发布，并汇总破坏性变更、废弃项、安全问题和大版本升级

### 🤝 多 Agent 团队

*多个 Agent 协同完成复杂的跨领域任务。*

*   [🧲 AI Competitor Intelligence Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_competitor_intelligence_agent_team/) - 基于竞争对手官网构建结构化竞争分析
*   [💲 AI Finance Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_finance_agent_team/) - 用约 20 行 Python 组成金融分析 Agent 团队
*   [🎨 AI Game Design Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_game_design_agent_team/) - 由多个游戏设计专家 Agent 协作生成完整游戏概念
*   [🧭 AG2 Adaptive Research Team](advanced_ai_agents/multi_agent_apps/agent_teams/ag2_adaptive_research_team/) - 基于 AG2 构建，支持路由与回退机制的 Agent 协作研究团队
*   [👨‍⚖️ AI Legal Agent Team (Cloud & Local)](advanced_ai_agents/multi_agent_apps/agent_teams/ai_legal_agent_team/) - 提供法律研究、合同分析与策略建议
*   [💼 AI Recruitment Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_recruitment_agent_team/) - 从简历筛选到面试安排的端到端招聘流程
*   [🏠 AI Real Estate Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_real_estate_agent_team) - 房产搜索、市场分析和推荐
*   [👨‍💼 AI Services Agency (CrewAI)](advanced_ai_agents/multi_agent_apps/agent_teams/ai_services_agency/) - 像数字服务公司一样评估并规划软件项目
*   [👨‍🏫 AI Teaching Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/ai_teaching_agent_team/) - 由多个教学 Agent 协作构建完整学习路径
*   [💻 Multimodal Coding Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/multimodal_coding_agent_team/) - 拍摄编程题照片，获得沙盒环境中的解决方案
*   [✨ Multimodal Design Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/multimodal_design_agent_team/) - 使用 Gemini 驱动的专家团队进行设计评审
*   [🎨 🍌 Multimodal UI/UX Feedback Agent Team](advanced_ai_agents/multi_agent_apps/agent_teams/multimodal_uiux_feedback_agent_team/) - 对落地页进行反馈，并自动生成改进版本
*   [🌏 AI Travel Planner Agent Team](/advanced_ai_agents/multi_agent_apps/agent_teams/ai_travel_planner_agent_team/) - 由 Agent 团队协作生成完整旅行计划

### 🗣️ 语音 AI Agents

*使用实时语音 API，实现语音输入与语音输出。*

*   [🗣️ AI Audio Tour Agent](voice_ai_agents/ai_audio_tour_agent/) - 根据位置、兴趣和节奏生成自助式语音导览
*   [📞 Customer Support Voice Agent](voice_ai_agents/customer_support_voice_agent/) - 基于自有文档提供有依据的语音客服回答
*   [🛡️ Insurance Claim Live Agent Team](voice_ai_agents/insurance_claim_live_agent_team/) - 使用 Gemini Live 实时完成保险理赔信息收集
*   [🔊 Voice RAG Agent (OpenAI SDK)](voice_ai_agents/voice_rag_openaisdk/) - 对 PDF 提问并直接听取语音回答
*   [🎙️ OpenSource Voice Dictation Agent (Wispr Flow clone)](https://github.com/akshayaggarwal99/jarvis-ai-assistant) <sub>↗ 外部项目</sub> - 开源语音听写工具，可将说话内容直接输入当前应用

### 🖼️ 生成式 UI 与 Agent 前端

*Agent 不只返回文本，还会渲染交互式 UI 组件，例如表单、卡片、图表和可编辑计划。*

*   [🗂️ Generative UI Starter Project](generative_ui_agents/generative-ui-starter-project/) - 通过聊天驱动的看板，你和 Agent 可以协同工作
*   [🪙 AI Financial Coach Agent](generative_ui_agents/ai-financial-coach-agent/) - 将预算、储蓄与债务计划渲染成交互式卡片
*   [📊 AI Dashboard Canvas Agent](generative_ui_agents/ai-dashboard-canvas-agent/) - 在聊天中描述 Dashboard，图表会自动在实时画布上生成
*   [🛠️ AI MCP App Builder](generative_ui_agents/ai-mcp-app-builder/) - 描述一个 MCP 应用，即可生成可运行的沙盒实例
*   [✈️ MCP Apps Generative UI Showcase](generative_ui_agents/mcp-apps-generative-ui-showcase/) - 展示可以渲染真实交互式 UI 的 MCP App，包括航班搜索
*   [🎛️ AI Shadcn Component Generator](generative_ui_agents/ai-shadcn-component-generator/) - 通过聊天生成可用于生产环境的 shadcn 组件
*   [🔍 AI Deep Research Agent](generative_ui_agents/ai-deep-research-agent/) - 每次工具调用都渲染为实时工作区卡片的研究 Agent

### 🎮 自主游戏 Agents

*能够端到端玩游戏的 Agent，负责推理、策略制定与实际操作。*

*   [🎮 AI 3D Pygame Agent](advanced_ai_agents/autonomous_game_playing_agent_apps/ai_3dpygame_r1/) - DeepSeek R1 编写 PyGame 代码，并由浏览器 Agent 实时运行
*   [♜ AI Chess Agent](advanced_ai_agents/autonomous_game_playing_agent_apps/ai_chess_agent/) - 白方 Agent 与黑方 Agent 对弈，并验证每一步是否合法
*   [🎲 AI Tic-Tac-Toe Agent](advanced_ai_agents/autonomous_game_playing_agent_apps/ai_tic_tac_toe_agent/) - 两个不同 LLM 按回合进行井字棋对战

### ♾️ MCP AI Agents

*通过 Model Context Protocol（MCP）连接外部工具与数据源的 Agent。*

*   [♾️ Browser MCP Agent](mcp_ai_agents/browser_mcp_agent/) - 通过 MCP 使用自然语言控制真实浏览器
*   [🐙 GitHub MCP Agent](mcp_ai_agents/github_mcp_agent/) - 使用自然语言探索和分析任意 GitHub 仓库
*   [📑 Notion MCP Agent](mcp_ai_agents/notion_mcp_agent) - 在终端中直接与 Notion 页面交互
*   [🌍 AI Travel Planner MCP Agent](mcp_ai_agents/ai_travel_planner_mcp_agent_team) - 基于实时 Airbnb 和 Google Maps 数据生成旅行计划
*   [🔀 Multi-MCP Agent Router](mcp_ai_agents/multi_mcp_agent_router/) - 多个专用 Agent，每个 Agent 连接不同 MCP Server
*   [🔌 OpenAI Remote MCP Tool Bridge](mcp_ai_agents/openai_remote_mcp_bridge/) - 将 OpenAI function calling 直接连接到远程 MCP Server

### 📀 RAG（检索增强生成）

*从基础检索链到 Agentic RAG 与多数据源 RAG 的各种检索管线。*

*   [🔥 Agentic RAG with Embedding Gemma](rag_tutorials/agentic_rag_embedding_gemma) - 使用 EmbeddingGemma 和 Llama 3.2 构建完全本地运行的 Agentic RAG
*   [🧐 Agentic RAG with Reasoning](rag_tutorials/agentic_rag_with_reasoning/) - 在检索过程中查看 Agent 的逐步推理过程
*   [📰 AI Blog Search (RAG)](rag_tutorials/ai_blog_search/) - 基于 LangGraph 对博客内容进行 Agentic 搜索
*   [🔍 Autonomous RAG](rag_tutorials/autonomous_rag/) - GPT-4o 基于 PDF 回答，证据不足时自动回退到 Web 搜索
*   [🔄 Contextual AI RAG Agent](rag_tutorials/contextualai_rag_agent/) - 托管式 RAG，从数据存储到有依据的聊天只需几分钟
*   [🔄 Corrective RAG (CRAG)](rag_tutorials/corrective_rag/) - 自动评估自身检索结果，并在回答前重新检索
*   [📎 Typed Agentic RAG with Pydantic AI](rag_tutorials/agentic_typed_rag_pydanticai/) - 返回带精确引用的结构化答案；证据不足时拒绝回答
*   [🐋 Deepseek Local RAG Agent](rag_tutorials/deepseek_local_rag_agent/) - 在本地文档上运行 DeepSeek 推理
*   [🤔 Gemini Agentic RAG](rag_tutorials/gemini_agentic_rag/) - 使用 Gemini Flash Thinking 进行查询改写并支持 Web 回退
*   [👀 Hybrid Search RAG (Cloud)](rag_tutorials/hybrid_search_rag/) - 将关键词搜索与向量搜索结合后交给 Claude
*   [🔄 Llama 3.1 Local RAG](rag_tutorials/llama3.1_local_rag/) - 与任意网页内容聊天，并可完全离线运行
*   [🖥️ Local Hybrid Search RAG](rag_tutorials/local_hybrid_search_rag/) - 所有组件均在本机运行的混合检索方案
*   [🧬 Multimodal Agentic RAG](rag_tutorials/multimodal_agentic_rag/) - 对文本、PDF、图片、音频和视频进行多模态检索，并返回带引用的答案
*   [🦙 Local RAG Agent](rag_tutorials/local_rag_agent/) - 使用 Llama 3.2 与 Qdrant，本地运行且无需 API Key
*   [🧩 RAG-as-a-Service](rag_tutorials/rag-as-a-service/) - 用不到 50 行代码实现面向生产环境的 RAG 服务
*   [✨ RAG Agent with Cohere](rag_tutorials/rag_agent_cohere/) - 使用 Command R7B 检索，并支持 Web 搜索回退
*   [⛓️ Basic RAG Chain](rag_tutorials/rag_chain/) - 最精简的检索增强生成管线，以药物研究为示例
*   [📠 RAG with Database Routing](rag_tutorials/rag_database_routing/) - 自动将每个问题路由到最适合的数据库
*   [🖼️ Vision RAG](rag_tutorials/vision_rag/) - 使用 Embed-4 对图片和 PDF 页面进行问答
*   [🩺 RAG Failure Diagnostics Clinic](rag_tutorials/rag_failure_diagnostics_clinic/) - 系统性诊断 RAG 管线为什么产生错误答案
*   [🕸️ Knowledge Graph RAG with Citations](rag_tutorials/knowledge_graph_rag_citations/) - 生成可验证来源归因的多跳答案

### 💾 带记忆能力的 LLM 应用

*能够跨会话记住对话内容和用户状态的 Agent 与聊天机器人。*

*   [💾 AI ArXiv Agent with Memory](advanced_llm_apps/llm_apps_with_memory_tutorials/ai_arxiv_agent_memory/) - 记住你的研究兴趣，并据此搜索论文
*   [🛩️ AI Travel Agent with Memory](advanced_llm_apps/llm_apps_with_memory_tutorials/ai_travel_agent_memory/) - 能记住用户偏好的旅行助手
*   [💬 Llama3 Stateful Chat](advanced_llm_apps/llm_apps_with_memory_tutorials/llama3_stateful_chat/) - 基于 Llama 3 的跨会话持久化聊天
*   [📝 LLM App with Personalized Memory](advanced_llm_apps/llm_apps_with_memory_tutorials/llm_app_personalized_memory/) - 能在多次对话之间保留上下文的聊天机器人
*   [🗄️ Local ChatGPT Clone with Memory](advanced_llm_apps/llm_apps_with_memory_tutorials/local_chatgpt_with_memory/) - 完全本地运行，并为每个用户维护独立记忆
*   [🧠 Multi-LLM Application with Shared Memory](advanced_llm_apps/llm_apps_with_memory_tutorials/multi_llm_memory/) - 多个不同模型共享同一份会话记忆

### 💬 与各种数据源聊天

*将任意数据源转换成自然语言聊天界面。*

*   [💬 Chat with GitHub (GPT & Llama3)](advanced_llm_apps/chat_with_X_tutorials/chat_with_github/) - 用约 30 行 RAG 代码对任意 GitHub 仓库进行问答
*   [📨 Chat with Gmail](advanced_llm_apps/chat_with_X_tutorials/chat_with_gmail/) - 直接对你的收件箱提问
*   [📄 Chat with PDF (GPT & Llama3)](advanced_llm_apps/chat_with_X_tutorials/chat_with_pdf/) - 经典 PDF 问答，用约 30 行 Python 实现
*   [📚 Chat with Research Papers (ArXiv) (GPT & Llama3)](advanced_llm_apps/chat_with_X_tutorials/chat_with_research_papers/) - 使用 GPT-4o 以对话方式探索 arXiv 论文
*   [📝 Chat with Substack](advanced_llm_apps/chat_with_X_tutorials/chat_with_substack/) - 与任意 Substack Newsletter 的历史文章进行对话
*   [📽️ Chat with YouTube Videos](advanced_llm_apps/chat_with_X_tutorials/chat_with_youtube_videos/) - 通过字幕内容对 YouTube 视频提问

### 🎯 LLM 优化工具

*在尽量不损失输出质量的前提下，降低 Token 使用量、上下文规模和 API 成本。*

*   [🎯 Toonify Token Optimization](advanced_llm_apps/llm_optimization_tools/toonify_token_optimization/) - 使用 TOON 格式将 LLM API 成本降低约 30–60%
*   [🧠 Headroom Context Optimization](advanced_llm_apps/llm_optimization_tools/headroom_context_optimization/) - 将 LLM API 成本降低约 50–90%

### 🔧 LLM 微调

*面向开源模型的端到端微调示例。*

*   [🦥 Gemma 3 Fine-tuning](advanced_llm_apps/llm_finetuning_tutorials/gemma3_finetuning/) - 使用 Unsloth 完成 4-bit LoRA 微调，代码简洁易读
*   [🦙 Llama 3.2 Fine-tuning](advanced_llm_apps/llm_finetuning_tutorials/llama3.2_finetuning/) - 约 30 行代码即可完成微调，并可免费在 Colab 运行

### 🧑‍🏫 AI Agent 框架速成教程

*深入介绍主流 Agent Framework 的核心概念和实践方式。*

*   [Google ADK Crash Course](ai_agent_framework_crash_course/google_adk_crash_course/) - 涵盖入门 Agent、结构化输出、工具（内置、函数、第三方、MCP）、记忆、回调、插件及多 Agent 模式，并且与模型无关
*   [OpenAI Agents SDK Crash Course](ai_agent_framework_crash_course/openai_sdk_crash_course/) - 涵盖入门 Agent、函数调用、结构化输出、工具、记忆、评测、handoff、swarm 编排及路由逻辑

---

<div align="center">

⭐ **[为原项目点 Star](https://github.com/Shubhamsaboo/awesome-llm-apps/stargazers)**，即可关注新模板更新。

<sub>
<!-- 保留这些链接。原项目 README 更新时，多语言页面会自动同步。 -->
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=de">Deutsch</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=es">Español</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=fr">français</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=ja">日本語</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=ko">한국어</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=pt">Português</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=ru">Русский</a> ·
<a href="https://www.readme-i18n.com/Shubhamsaboo/awesome-llm-apps?lang=zh">中文</a>
</sub>

<sub>Apache-2.0 · 详见 <a href="LICENSE">LICENSE</a> · 可以 Fork、部署并用于商业项目。</sub>

</div>
