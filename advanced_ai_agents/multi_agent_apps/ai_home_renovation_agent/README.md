# 🏚️ 🍌 AI 家居翻新规划 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-ai-home-renovation-planner-agent-using-nano-banana)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 Google ADK 构建的多 Agent 系统，可分析你的房间照片、生成个性化翻新方案，并利用 Gemini 3 Flash 与 Gemini 3 Pro 的多模态能力生成逼真的装修效果图。

## 功能特性

- **🔍 智能图像分析**：上传房间照片和参考图片后，Agent 会自动识别并分析内容
- **🎨 写实效果图生成**：使用 Gemini 3 Pro 生成专业级翻新后空间效果图
- **💰 预算感知规划**：根据预算限制调整装修建议
- **📊 完整项目路线图**：提供工期、预算拆分、施工人员清单和行动检查表
- **🤖 多 Agent 编排**：展示 Coordinator/Dispatcher + Sequential Pipeline 模式
- **✏️ 迭代式优化**：可根据反馈继续修改已经生成的装修效果图

## 工作原理

系统采用 **Coordinator/Dispatcher 模式**，包含三个专用 Agent：

1. **Visual Assessor（视觉评估 Agent）** 📸
   - 分析上传的房间照片，包括布局、现状和尺寸
   - 从参考图片中提取装修风格
   - 估算成本并识别重点改造机会

2. **Design Planner（设计规划 Agent）** 🎨
   - 根据预算制定合适的设计方案
   - 明确材料、颜色和固定装置
   - 优先安排效果最明显的改造项目

3. **Project Coordinator（项目协调 Agent）** 🏗️
   - 生成完整的翻新路线图
   - 创建翻新后的写实效果图
   - 提供预算拆分、工期和具体行动步骤

## 快速开始

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/advanced_ai_agents/multi_agent_apps/ai_home_renovation_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

3. **配置 API Key**

```bash
export GOOGLE_API_KEY="your_gemini_api_key"
```

或者创建 `.env` 文件：

```text
GOOGLE_API_KEY=your_gemini_api_key
```

4. **启动 ADK Web**

```bash
cd multi_agent_apps
adk web
```

5. **打开浏览器**，并选择 `ai_home_renovation_agent`

## 使用示例

### 场景 1：当前房间 + 预算

```text
[上传你的厨房照片]
“预算 5000 美元，这里可以怎么改？”
```

→ Agent 会分析空间、给出预算友好的改造建议，并生成效果图。

### 场景 2：当前房间 + 灵感参考图

```text
[上传图片 1：你的厨房]
[上传图片 2：Pinterest 参考图]
“把我的厨房改成这种风格，大概需要多少钱？”
```

→ Agent 会从参考图中提取风格并应用到当前房间，同时给出预算和效果图。

### 场景 3：纯文本描述

```text
“翻新我的 10x12 英尺厨房，使用橡木橱柜和层压板台面。
希望做成现代农舍风格，白色 Shaker 橱柜。预算：3 万美元。”
```

→ Agent 会根据描述创建设计方案并生成装修效果图。

### 场景 4：迭代优化

```text
[初始效果图生成后]
“把橱柜从白色改成奶油色”
“在中岛上方增加吊灯”
“把地板换成更浅的橡木色”
```

→ Agent 会根据反馈继续调整效果图。

## 示例提示词

- “我想翻新一个 8x12 英尺的小型狭长厨房，现在是 90 年代的橡木橱柜。我喜欢现代农舍风格，预算 2.5 万美元。”
- “我的主浴室只有 5x8 英尺，现在浴缸区域很拥挤。我想改成带步入式淋浴的 SPA 风格，预算 1.5 万美元。”
- “把普通卧室改成舒适的休息空间，考虑增加特色墙和更换地板，预算 1.2 万美元。”

## 工具与能力

- **google_search**：查找装修成本、材料和趋势
- **estimate_renovation_cost**：按照房间类型和改造范围估算费用
- **calculate_timeline**：估算项目工期
- **generate_renovation_rendering**：生成写实装修效果图
- **edit_renovation_rendering**：根据反馈调整效果图
- **Versioned artifacts**：自动对所有效果图进行版本追踪

## 多 Agent 模式

本项目展示了 **Coordinator/Dispatcher + Sequential Pipeline** 架构：

```text
Coordinator（根 Agent）
    ├── Info Agent（快速问答）
    └── Planning Pipeline（顺序执行）
          ├── Visual Assessor（图像分析）
          ├── Design Planner（设计规格）
          └── Project Coordinator（效果图 + 项目路线图）
```

**为什么采用这种模式？**

- 高效：只运行实际需要的工作流
- 模块化：每个 Agent 职责清晰
- 易扩展：方便增加新的功能
- 面向生产：属于较典型的真实 Agentic System 架构模式
