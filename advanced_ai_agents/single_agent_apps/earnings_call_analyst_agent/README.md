# 📡 财报电话会议分析 Agent

这是一个面向投资者的财报电话会议分析工具，可将任意 YouTube 财报电话会议转换成与视频播放进度同步的分析工作区。只需粘贴会议视频 URL，一边观看视频，一边由 ADK Agent 提取关键数字、语气变化、财报文件背景以及可能影响市场的重要信息。

该项目围绕真实的财报分析工作流设计：不必等电话会议结束后再单独阅读文字稿，而是可以在管理层发言过程中，通过 Agent 研究层同步查看分析，并且每条洞察都与触发它的原始引文保持关联。

![📡 财报电话会议分析 Agent 架构](assets/earnings-call-analyst-agent-architecture.png)

## 功能特性

### Agent 驱动的电话会议研究

- 根据 YouTube 元数据和文字稿开头识别公司、股票代码、财报周期及同行公司
- 结合 SEC 文件和当前市场背景构建研究资料包
- 使用带 Google Search Grounding 的 ADK 新闻 Agent 获取当前市场信息
- 对尚未获取到的背景信息直接隐藏对应区域，而不是展示空白研究面板

### 基于原始引文的信号检测

- 仅当文字稿中出现真实的投资者信号时才创建分析卡片
- 每张卡片都关联触发该分析的准确引文和时间戳
- 自动过滤问候语、安全港声明以及泛化的积极表述
- 当视频播放到相关位置时，再显示对应分析卡片

### 财报情报卡片

- 标记财务指标、利润率压力、业绩指引、需求评论、定价、现金流和资本支出等信号
- 在有足够证据时，将公司自身信息与同行或行业背景分开呈现
- 识别 CFO 的保守措辞、信心变化、防御性表达以及异常具体的语言
- 仅在有助于说明结论时添加简洁表格或图表摘要

### 字幕与音频容错能力

- 有 YouTube 字幕时优先使用字幕，以获得准确时间戳
- 对没有字幕的视频，使用 ADK 驱动的音频转录作为后备方案
- 将自动生成的分析卡片重新对齐到最近的字幕片段，确保视频与引文保持同步
- 保证文字稿、研究资料包和分析卡片都基于同一条时间线

## 如何开始

该 Agent 位于：`advanced_ai_agents/single_agent_apps/earnings_call_analyst_agent`

1. 克隆 GitHub 仓库：

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/single_agent_apps/earnings_call_analyst_agent
```

2. 安装所需依赖：

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

3. 配置 Vertex AI 或 Gemini API Key：

```bash
cp .env.example .env
```

如果使用 Vertex AI / Google Cloud 认证：

```bash
GOOGLE_GENAI_USE_VERTEXAI=True
GOOGLE_CLOUD_PROJECT=your-google-cloud-project-id
GOOGLE_CLOUD_LOCATION=global
```

如果使用 Gemini API Key 认证：

```bash
GOOGLE_GENAI_USE_VERTEXAI=False
GOOGLE_API_KEY=your-google-api-key
```

4. 启动 FastAPI 应用：

```bash
PYTHONPATH=.. python -m uvicorn earnings_call_analyst_agent.live_demo.server:app --host 127.0.0.1 --port 4188
```

5. 打开应用：

```text
http://127.0.0.1:4188
```

粘贴 YouTube 财报电话会议 URL。应用会先构建研究资料包，然后随着视频播放到对应引文位置，逐步显示分析卡片。
