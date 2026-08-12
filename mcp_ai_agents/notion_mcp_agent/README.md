# 📑 Notion MCP 智能体

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-terminal-based-notion-agent-with-mcp)，从零开始学习如何构建该项目，并获得详细代码讲解、说明和最佳实践。**

这是一个基于终端的 Notion Agent，通过 Notion MCP（Model Context Protocol，模型上下文协议）服务器，让你可以使用自然语言与自己的 Notion 页面交互。

## 功能

- 通过命令行界面与 Notion 页面交互
- 对 Notion 页面执行更新、插入和读取操作
- 创建和编辑区块、列表、表格以及其他 Notion 结构
- 为区块添加评论
- 搜索特定信息
- 记住多轮对话上下文
- 提供会话管理，实现持续对话

## 环境要求

- Python 3.10+
- 具有管理员权限的 Notion 账号
- Notion Integration Token
- OpenAI API Key

## 安装

1. 克隆仓库
2. 安装所需 Python 包：

```bash
pip install -r requirements.txt
```

3. 安装 Notion MCP Server（运行应用时会自动完成）

## 配置 Notion Integration

### 创建 Notion Integration

1. 打开 [Notion Integrations](https://www.notion.so/my-integrations)
2. 点击“New integration”
3. 为 Integration 命名，例如“Notion Assistant”
4. 选择所需权限（读取和写入内容）
5. 提交并复制“Internal Integration Token”

### 将 Notion 页面共享给 Integration

1. 打开你的 Notion 页面
2. 点击页面右上角的三个点（⋮）
3. 从下拉菜单中选择“Add connections”
4. 在搜索框中搜索你的 Integration 名称
5. 点击该 Integration，将其添加到页面
6. 在弹出的对话框中点击“Confirm”确认

也可以通过“Share”按钮进行共享：
1. 点击右上角“Share”
2. 在共享对话框中搜索你的 Integration 名称（前面带“@”）
3. 点击该 Integration 将其添加
4. 点击“Invite”，授予其页面访问权限

以上两种方式都会让 Integration 获得该页面及其内容的完整访问权限。

### 查找 Notion Page ID

1. 在浏览器中打开你的 Notion 页面
2. 复制页面 URL，格式类似：
   `https://www.notion.so/workspace/Your-Page-1f5b8a8ba283...`
3. ID 是最后一个短横线之后、查询参数之前的部分
   示例：`1f5b8a8bad058a7e39a6`

## 配置

可以通过环境变量配置 Agent：

- `NOTION_API_KEY`：Notion Integration Token
- `OPENAI_API_KEY`：OpenAI API Key
- `NOTION_PAGE_ID`：Notion 页面 ID

也可以直接在脚本中设置这些值。

## 使用方法

从命令行运行 Agent：

```bash
python notion_mcp_agent.py
```

启动后，Agent 会提示你输入 Notion Page ID。你可以：
1. 在提示符中输入页面 ID
2. 直接按 Enter，使用默认页面 ID（如果已配置）
3. 将页面 ID 作为命令行参数直接传入，从而跳过提示：

```bash
python notion_mcp_agent.py your-page-id-here
```

### 对话流程

每次启动 Agent 时，都会创建唯一的用户 ID 和会话 ID，用于保持对话上下文。这样 Agent 就可以记住之前的交互，并在关闭和重新启动应用后继续保持连贯对话。

你可以随时输入 `exit`、`quit`、`bye` 或 `goodbye` 退出会话。

## 示例查询

- “我的 Notion 页面里有什么？”
- “添加一个新段落，内容为‘今天的会议记录’”
- “创建一个包含三项的项目符号列表：Apple、Banana、Orange”
- “给第一段添加评论：‘看起来不错！’”
- “搜索所有与会议相关的内容”
- “总结到目前为止我们的对话”
