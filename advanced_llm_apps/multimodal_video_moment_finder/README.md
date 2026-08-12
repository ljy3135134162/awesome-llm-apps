# 🎬 多模态视频片段查找器

<p align="center">
  <img src="assets/hero_banner.png" alt="Multimodal Video Moment Finder" width="700">
</p>

通过图片或文字查找视频中的任意时刻。你可以上传一张截图来定位它在视频中出现的位置，也可以直接用文字描述场景。整个过程基于纯视觉匹配，无需转录。

底层使用 **Gemini Embedding 2** 实现原生跨模态搜索。

## 工作原理

1. **上传视频** —— 使用 ffmpeg 按 1fps 提取视频帧
2. **为每一帧生成向量** —— 使用 `gemini-embedding-2-preview` 原生生成嵌入
3. **按图片搜索** —— 对上传图片生成嵌入，与全部视频帧计算余弦相似度
4. **按文本搜索** —— 对文字描述生成嵌入，并与视频帧进行跨模态匹配
5. **跳转到对应时刻** —— 点击任意结果，即可从相应时间戳开始播放视频

无需语音转录、无需字幕、无需 OCR。Embedding 模型会直接理解视觉内容。

## 技术栈

- **后端**：FastAPI + Gemini Embedding 2 + ChromaDB
- **前端**：Next.js（深色主题、分栏布局）
- **视频帧提取**：ffmpeg（1fps）
- **视频帧描述**：Gemini 3 Flash
- **模型**：`gemini-embedding-2-preview`（嵌入）、`gemini-3-flash-preview`（描述）

## 项目结构

```text
advanced_llm_apps/multimodal_video_moment_finder/
├── backend/
│   ├── server.py           # FastAPI 服务，提供上传、搜索和视频管理接口
│   ├── video_store.py      # 视频处理、帧提取、嵌入生成和 ChromaDB 存储
│   └── requirements.txt    # Python 依赖
├── frontend/
│   ├── app/
│   │   ├── page.tsx        # 主界面：视频上传、图片/文本搜索、结果播放
│   │   ├── layout.tsx      # 根布局
│   │   └── globals.css     # 全局样式
│   ├── package.json
│   ├── next.config.ts
│   └── tsconfig.json
└── README.md
```

## 配置

### 前置条件

- Python 3.10+
- Node.js 18+
- 已安装 ffmpeg（`brew install ffmpeg` 或 `apt install ffmpeg`）
- [Google AI API Key](https://aistudio.google.com/apikey)

### 后端

```bash
cd advanced_llm_apps/multimodal_video_moment_finder/backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

export GOOGLE_API_KEY="your-api-key"
python server.py
```

后端运行于 `http://localhost:8890`。

### 前端

```bash
cd advanced_llm_apps/multimodal_video_moment_finder/frontend
npm install
echo 'NEXT_PUBLIC_API_URL=http://localhost:8890' > .env.local
npm run dev
```

前端运行于 `http://localhost:3000`。

## 使用方法

1. 打开 `http://localhost:3000`
2. 上传一个视频（ffmpeg 支持的任意格式）
3. 等待视频帧提取并生成嵌入（每秒 1 帧）
4. 开始搜索：
   - **图片搜索**：上传截图或照片，查找它出现的位置
   - **文本搜索**：描述场景，例如“舞台上的人”或“城市航拍画面”
5. 点击任意结果，即可跳转到视频中的对应时刻

## API 端点

| 方法 | 端点 | 说明 |
|--------|----------|-------------|
| POST | `/upload-video` | 上传并索引视频 |
| POST | `/find-moment` | 按图片搜索（multipart form） |
| POST | `/find-moment-text` | 按文本描述搜索 |
| GET | `/videos` | 列出已索引视频 |
| DELETE | `/videos/{id}` | 删除视频 |
| GET | `/health` | 状态检查 |

## 架构

<p align="center">
  <img src="assets/architecture_diagram.png" alt="Architecture Diagram" width="600">
</p>

## 核心思路

Gemini Embedding 2 可以原生将图片和文本映射到同一个向量空间。这意味着，你既可以通过另一张图片搜索某个视觉片段，也可以直接使用文字描述进行搜索，而无需先经过图像描述、字幕生成或语音转录等中间步骤。模型会直接理解视频帧中的内容。
