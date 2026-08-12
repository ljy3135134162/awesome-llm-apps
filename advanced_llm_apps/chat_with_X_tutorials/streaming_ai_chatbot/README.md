# 流式 AI 聊天机器人

这是一个使用 Motia 框架实现的极简示例，展示如何进行**实时 AI 流式输出**以及**对话状态管理**。

![streaming-ai-chatbot](docs/images/streaming-ai-chatbot.gif)

## 🚀 功能

- **实时 AI 流式输出**：使用 OpenAI Streaming API 按 Token 逐步生成响应
- **实时状态管理**：随着消息历史变化，实时更新对话状态
- **事件驱动架构**：清晰的 API → Event → Streaming Response 流程
- **极低复杂度**：只用 3 个核心文件即可实现完整功能

## 📁 架构

```
streaming-ai-chatbot/
├── steps/
│   ├── conversation.stream.ts    # 实时对话状态
│   ├── chat-api.step.ts          # 简单聊天 API 端点
│   └── ai-response.step.ts       # AI 流式响应处理器
├── package.json                  # 依赖
├── .env.example                  # 配置模板
└── README.md                     # 本文件
```

## 🛠️ 配置

### 安装与启动

```bash
# 克隆仓库
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_llm_apps/chat_with_X_tutorials/chat_with_llms

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

### 配置 OpenAI API

```bash
cp .env.example .env
# 编辑 .env，并添加你的 OpenAI API Key
```

**打开 Motia Workbench**：
访问 `http://localhost:3000` 与聊天机器人进行交互。

## 🔧 使用方法

### 发送聊天消息

**POST** `/chat`

```json
{
  "message": "Hello, how are you?",
  "conversationId": "optional-conversation-id"
}
```

其中 `conversationId` 为可选项。如果未提供，系统会自动创建新的对话。

**响应：**

```json
{
  "conversationId": "uuid-v4",
  "message": "Message received, AI is responding...",
  "status": "streaming"
}
```

随着 AI 处理消息，响应状态会持续更新。可能的状态包括：

- `created`：初始消息状态
- `streaming`：AI 正在生成响应
- `completed`：响应已完成并包含完整消息

完成后，响应中的处理提示信息会被实际的 AI 回复替换。

### 实时状态更新

对话 State Stream 会在 AI 生成回复时持续提供实时更新：

- **用户消息**：立即存储，并设置 `status: 'completed'`
- **AI 响应**：以 `status: 'streaming'` 开始，实时更新内容，最终变为 `status: 'completed'`

## 🎯 展示的关键概念

### 1. **Streaming API 集成**

```typescript
const stream = await openai.chat.completions.create({
  model: 'gpt-4o-mini',
  messages: [...],
  stream: true, // 启用流式输出
})

for await (const chunk of stream) {
  // 每收到一个 Token 就更新状态
  await streams.conversation.set(conversationId, messageId, {
    message: fullResponse,
    status: 'streaming',
    // ...
  })
}
```

### 2. **实时状态管理**

```typescript
export const config: StateStreamConfig = {
  name: 'conversation',
  schema: z.object({
    message: z.string(),
    from: z.enum(['user', 'assistant']),
    status: z.enum(['created', 'streaming', 'completed']),
    timestamp: z.string(),
  }),
  baseConfig: { storageType: 'state' },
}
```

### 3. **事件驱动流程**

```typescript
// API 发出事件
await emit({
  topic: 'chat-message',
  data: { message, conversationId, assistantMessageId },
})

// 事件处理器订阅并处理事件
export const config: EventConfig = {
  subscribes: ['chat-message'],
  // ...
}
```

## 🌟 为什么这个示例值得关注

这个示例只使用 **3 个文件**，就展示了 Motia 的核心能力：

- **轻松实现流式输出**：实时 AI 响应并自动更新状态
- **类型安全事件**：从 API 到事件处理器实现端到端类型安全
- **内置状态管理**：无需额外引入外部状态管理库
- **可扩展架构**：事件驱动设计可以随着业务需求自然扩展

它非常适合用来展示 Motia 如何将复杂的实时应用开发变得简单且易于维护。

## 🔑 环境变量

- `OPENAI_API_KEY`：你的 OpenAI API Key（必需）
