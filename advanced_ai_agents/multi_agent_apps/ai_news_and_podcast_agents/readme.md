# 🦉 Beifong：无垃圾信息的个性化资讯与播客系统

![image](https://github.com/user-attachments/assets/b2f24f12-6f80-46fa-aa31-ee42e17765b1)

Beifong 用于管理你信任的文章来源和社交媒体信息源，并基于你主动筛选和认可的内容生成播客。它覆盖从数据采集、内容分析，到播客脚本、音频与视觉素材生成的完整流程。

▶️ [观看高清演示视频](https://www.canva.com/design/DAGoUfv8ICM/Oj-vJ19AvZYDa2SwJrCWKw/watch?utm_content=D[…]hare&utm_medium=link2&utm_source=uniquelinks&utlId=h2508379667)

▶️ [在 YouTube 上观看演示](https://youtu.be/dB8FZY3x9EY)

🔗 [博客](https://arun477.github.io/posts/beifong_podcast_generator/)

## 目录

- [快速开始](#快速开始)
- [如何使用 Beifong](#如何使用-beifong)
- [内容处理系统](#内容处理系统)
- [AI Agent 与工具](#ai-agent-与工具)
- [网页搜索与浏览器自动化](#网页搜索与浏览器自动化)
- [社交媒体监控](#社交媒体监控)
- [音频与语音生成](#音频与语音生成)
- [集成](#集成)
- [数据存储与文件管理](#数据存储与文件管理)
- [部署与访问方式](#部署与访问方式)
- [云端版本](#云端版本)
- [故障排查](#故障排查)
- [更新](#更新)

## 快速开始

### 系统要求

安装 Beifong 前，请确保具备：

- Python 3.11+
- Redis Server
- OpenAI API Key
- ElevenLabs API Key（可选）

### 初始安装

```bash
# 克隆仓库
git clone https://github.com/arun477/beifong.git
cd beifong

# 创建虚拟环境
cd beifong
python -m venv venv
source venv/bin/activate

# 安装依赖
pip install -r requirements.txt

# 安装浏览器组件
python -m playwright install

# 可选但推荐：下载演示数据
cd beifong
python bootstrap_demo.py
```

`bootstrap_demo.py` 会向系统中填充示例数据、精选信息源以及相关素材。

### 环境配置

在 `/beifong` 目录创建 `.env` 文件：

```env
OPENAI_API_KEY=your_openai_api_key
ELEVENSLAB_API_KEY=your_elevenlabs_api_key  # 可选
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_DB=0
```

### 启动应用

请在不同终端中分别启动服务。首次运行时应先启动 `python main.py`，因为系统需要执行数据库初始化。

⚠️ 每个终端启动脚本前都应先激活虚拟环境。

```bash
source venv/bin/activate
```

```bash
# 终端 1：启动主后端
cd beifong
python main.py

# 终端 2：启动调度器
cd beifong
python -m scheduler

# 终端 3：启动聊天 Worker
cd beifong
python -m celery_worker

# 检查 Redis
redis-cli ping
```

#### 可选：前端开发模式

```bash
cd web
npm install
npm start
```

## 如何使用 Beifong

### 三种使用方式

Beifong 支持以下交互模式：

1. **交互式 Web UI**：用于内容管理和播客生成。
2. **API 集成**：供自定义应用和自动化工作流调用。
3. **自动调度**：创建周期性任务，自动完成内容采集和处理。

## 内容处理系统

### 内置内容处理器

Beifong 针对不同数据来源提供了多种专用处理器：

- **RSS Feed Processor**：监控 RSS 源中的新文章和内容。
- **URL Content Processor**：提取并处理网页内容。
- **AI Content Analyzer**：分类、总结并分析内容质量。
- **Vector Embedding Processor**：将内容转换为可搜索的向量表示。
- **FAISS Search Indexer**：构建内容检索索引。
- **Podcast Script Generator**：根据精选内容生成完整播客脚本。
- **X.com Social Processor**：抓取并处理 X.com 信息流。
- **Facebook Social Processor**：抓取并处理 Facebook 信息流。

### 创建自定义内容处理器

#### 第 1 步：创建处理器模块

```python
# processors/my_custom_processor.py
def process_custom_task(parameter1=None, parameter2=None):
    # 在这里实现处理逻辑
    stats = {"processed": 0, "success": 0, "errors": 0}
    return stats

if __name__ == "__main__":
    stats = process_custom_task()
    print(f"Processed: {stats['processed']}, Success: {stats['success']}")
```

#### 第 2 步：注册处理器

在 `models/tasks_schemas.py` 中加入处理器：

```python
class TaskType(str, Enum):
    # Existing task types...
    my_custom_processor = "my_custom_processor"

TASK_TYPES = {
    # Existing types...
    "my_custom_processor": {
        "name": "My Custom Processor",
        "command": "python -m processors.my_custom_processor",
        "description": "Performs custom processing task",
    },
}
```

#### 第 3 步：部署处理器

通过 API 或 Web UI 创建一个使用该自定义处理器类型的新任务即可。

## AI Agent 与工具

### Agent 架构概览

Beifong 的 AI 系统基于 [agno](https://github.com/agno-agi/agno) 构建，包括：

- **搜索工具**：语义搜索、关键词搜索以及基于浏览器的网页研究。
- **内容生成工具**：自动生成脚本、Banner 和音频。
- **持久化 Session 状态**：在多轮交互中保持上下文。
- **工具编排**：自动管理多步骤工作流。

### 添加自定义工具

```python
# tools/my_custom_tool.py
from agno.agent import Agent

def my_custom_tool(agent: Agent, param1: str, param2: str) -> str:
    """Tool description here"""
    agent.session_state["my_key"] = "my_value"
    result = f"Processed {param1} and {param2}"
    return result
```

然后在 `services/celery_tasks.py` 中注册：

```python
from tools.my_custom_tool import my_custom_tool

tools = [my_custom_tool]
```

### 配置 Agent 行为

Agent 的主要指令和行为可在 `db/agent_config_v2.py` 中调整。修改时应保留核心流程阶段，避免破坏现有工作流。

## 网页搜索与浏览器自动化

Beifong 的搜索 Agent 通过 [browseruse](https://browser-use.com/) 获得完整浏览器自动化能力，可以完成网页研究、页面交互和自动数据采集。

### 搜索指令示例

你可以向 Agent 提交类似任务：

- “进入我的 X.com，收集最有价值且正面的信息流。”
- “浏览 Reddit，查找本周关于 AI 发展的讨论。”
- “在 LinkedIn 中查找近期的数据科学趋势帖子。”
- “访问新闻网站，收集可再生能源相关文章。”

Agent 会自动导航网站、操作页面元素并提取所需信息。

### 社交媒体登录 Session

对于 X.com、Facebook、LinkedIn 等需要登录的网站，需要预先建立已登录 Session：

1. 进入 Beifong Web 界面的 **Social** 标签页。
2. 在 Setup 区域点击 `Setup Session`。
3. 系统会打开浏览器窗口：
   - 正常登录社交媒体账号；
   - 完成必要验证；
   - 登录完成后关闭浏览器。
4. Beifong 后续会复用该认证 Session 执行自动搜索。

### 高级持久化 Session 配置

默认浏览器 Session 保存在：

```text
browsers/playwright_persistent_profile_web
```

如果需要指定持久化路径，可修改 `tools/web_search`，调用 `db/config.py` 中的 `get_browser_session_path()`。

注意事项：

- 避免多个进程同时使用同一个浏览器 Session。
- Social Monitor Processor 通常会使用 `get_browser_session_path()` 返回的路径。
- 如果手动使用持久化 Session，建议关闭 Voyager 区域中的冲突型社交媒体监控任务。
- 后续版本计划将不同账户的 Session 分离。

若登录状态失效，可重新执行 Social 标签页中的 Session 设置流程。遇到认证异常时，也可以清理浏览器数据，并确保同一 Session 同时只由一个进程访问。

## 社交媒体监控

### 当前支持的平台

- **X.com（Twitter）**：采集并分析你的社交媒体 Feed。
- **Facebook.com**：监控 Facebook Timeline 和相关内容。

### 设置定时 Feed 采集

1. 进入 Web UI 的 **Voyager** 标签页。
2. 创建社交媒体监控定时任务。
3. 设置采集频率。
4. 选择 X.com 或 Facebook.com Processor。

### 查看 AI 洞察

采集完成后，在 **Social** 标签页可以查看每条内容的 AI 分析，包括：

- 情感分析
- 主题分类
- 互动表现分析
- 相关性评分

### 配置自定义 Feed

可在 `/tools/social/` 目录修改社交媒体处理器中的 URL，例如：

- 监控指定 X.com 用户；
- 监控指定 Facebook Page；
- 跟踪特定 Hashtag 或主题。

### 添加新的社交媒体平台

目前内置支持：

- X.com
- Facebook.com

同样的框架还可以扩展到：

- LinkedIn
- Reddit
- 其他平台

其他平台通常需要自行编写 Scraper 或接入对应 API。后续版本计划增加更多预置 Connector，并支持每个平台管理多个账户。

### 调度最佳实践

⚠️ 多个社交媒体抓取任务不要同时执行，因为当前它们会共享同一个持久化浏览器 Session。

建议：

- 错开 X.com 与 Facebook.com 的运行时间；
- 在不同任务间预留足够处理间隔；
- 观察每个任务的执行时长，避免任务重叠。

例如：

- X.com：每 2 小时的整点运行；
- Facebook.com：每 2 小时的第 30 分钟运行。

未来版本计划为不同社交媒体账户创建独立 Session，以支持并行采集。

## 音频与语音生成

### 支持的 TTS 引擎

**商业服务：**

- OpenAI TTS
- ElevenLabs

**开源方案：**

- Kokoro

### 添加新的语音引擎

可以通过 `utils` 目录中的 `tts_selector` Engine 接口扩展新的 TTS 后端。

可考虑的开源方案包括：

- [Dia TTS](https://yummy-fir-7a4.notion.site/dia)
- [CSM](https://github.com/SesameAILabs/csm)
- [Orpheus-TTS](https://github.com/canopyai/Orpheus-TTS)

## 集成

### Slack 集成

Beifong 可以直接接入 Slack。用户可以在 Slack Workspace 中与 AI Agent 对话，每次会话会创建独立 Thread，以持续维护上下文。

主要能力：

- 在 Slack Channel 中直接与 BeifongAI 交互。

### 配置 Slack App

Beifong 使用 Slack Socket Mode。

#### 第 1 步：创建 Slack App

1. 访问 [Slack API Apps](https://api.slack.com/apps)，点击 `Create New App`。
2. 选择 `From scratch`，填写：
   - App Name：`BeifongAI` 或其他名称；
   - Workspace：目标 Slack Workspace。
3. 打开左侧 `Socket Mode`，启用 Socket Mode。
4. 创建具有 `connections:write` Scope 的 App-Level Token。
5. 保存该 Token，作为 `SLACK_APP_TOKEN`。

#### 第 2 步：配置 Bot User

1. 进入 `OAuth & Permissions`。
2. 在 `Bot Token Scopes` 中添加后文所需权限。
3. 点击 `Install to Workspace` 并授权。
4. 复制 `Bot User OAuth Token`，作为 `SLACK_BOT_TOKEN`。

#### 第 3 步：启用 Event Subscriptions

1. 进入 `Event Subscriptions`。
2. 打开 `Enable Events`。
3. 添加后文列出的 Bot Events。

### Slack 所需权限

#### Bot Token Scopes

- `app_mentions:read`：读取对 @BeifongAI 的 Mention。
- `assistant:write`：允许 BeifongAI 作为 App Agent 工作。
- `channels:history`：读取已加入公共频道中的消息。
- `channels:read`：读取 Workspace 公共频道基本信息。
- `chat:write`：以 @BeifongAI 身份发送消息。
- `files:read`：读取可访问频道中的文件。
- `files:write`：上传、编辑和删除文件。
- `im:read`：读取直接消息基本信息。
- `im:write`：主动发起直接消息。

#### Bot Events

- `app_mention`：监听对应用/Bot 的 Mention。
  - 所需 Scope：`app_mentions:read`
- `message.channels`：监听频道消息。
  - 所需 Scope：`channels:history`

### Slack 环境变量

在 `/beifong/.env` 中加入：

```env
OPENAI_API_KEY=your_openai_api_key
ELEVENSLAB_API_KEY=your_elevenlabs_api_key  # 可选

SLACK_BOT_TOKEN=xoxb-your-bot-user-oauth-token
SLACK_APP_TOKEN=xapp-your-app-level-token

REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_DB=0
```

### 运行 Slack 集成

首先确保 Slack App 已安装到 Workspace，并将 BeifongAI 添加到需要使用的 Channel。也可以直接向 BeifongAI 发送私信。

然后运行：

```bash
cd beifong
source venv/bin/activate
python -m integrations.slack.chat
```

### 在 Slack 中使用

- 在频道中 Mention `@BeifongAI` 即可开始对话。
- 每次 Mention 会建立新的 Thread，用于维持上下文。
- 示例：`@BeifongAI 帮我分析一下最近关于 AI 发展的新闻。`

参考：[Slack Socket Mode API](https://api.slack.com/apis/socket-mode)

## 数据存储与文件管理

### 数据库存储

应用数据库统一保存在 `databases` 目录，便于管理和备份。

### 媒体素材存储

生成的播客、音频文件和视觉素材保存在 `podcasts` 目录。

### 管理存储增长

如果媒体资源不断增加，可考虑：

**云存储：**

- 使用 `s3fs` 将 S3 Bucket 挂载为本地目录。
- 通过 `.env` 自定义存储路径，将媒体文件放到更大磁盘。

**自动清理：**

- 定期归档旧播客节目。
- 自动删除临时录音和未使用素材。
- 针对不同类型内容配置 Retention Policy。

**容量监控：**

- 持续监控磁盘使用率。
- 针对容量阈值设置告警。

后续版本计划提供更完善的存储管理能力和云存储 Connector。

## 部署与访问方式

### 局域网访问

```bash
cd beifong
python main.py --host 0.0.0.0 --port 7000
```

这样同一局域网中的其他设备即可通过该机器 IP 地址访问 Beifong。

### 远程访问

#### SSH 端口转发

```bash
ssh -L 7000:localhost:7000 username@your-server-ip
```

#### Ngrok Tunnel

```bash
ngrok http 7000
```

Ngrok 会创建一个临时公网 URL，并将流量转发至本地 Beifong 实例。

### 安全性

当前 Beifong 尚未内置身份认证层，计划在后续版本加入 Authentication。

## 云端版本

### Beifong Cloud 规划

即将推出：

- Beifong Cloud 云端版本
- 更多社交媒体 Connector
- 更多模型/API 选项，包括 Claude、Gemini、OpenAI、Ollama
- 更多播客风格与自定义能力
- 更多语音选项
- 更完善的数据采集与存储管理
- 身份认证层

## 故障排查

### Kokoro 安装失败

如果 Kokoro 导致依赖安装失败，可以暂时跳过。它仅在使用 Kokoro 作为 TTS 引擎时需要。

参考：https://github.com/hexgrad/kokoro

### Browseruse 安装问题

如果 browseruse 安装或运行异常，请确认 Playwright 已正确安装。浏览器自动化能力依赖 Playwright。

参考：https://github.com/browser-use/browser-use

### FAISS 安装失败

如果 FAISS 安装失败，可以在不需要语义搜索功能的情况下直接跳过。FAISS 仅用于语义搜索相关功能。

参考：https://github.com/facebookresearch/faiss

### 基于浏览器的数据采集异常

部分数据采集功能依赖浏览器自动化。在 Server 环境中，如果浏览器环境未正确配置，这些功能可能无法正常运行。Beifong 主体仍可运行，但依赖浏览器的功能可能受到影响。

## 更新

🚀 **[Beifong 项目仓库](https://github.com/arun477/beifong)**
