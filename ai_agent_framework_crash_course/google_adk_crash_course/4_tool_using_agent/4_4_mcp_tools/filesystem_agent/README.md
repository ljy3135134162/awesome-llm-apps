# 📁 文件系统 Agent：MCP 集成

本示例演示如何使用 `MCPToolset` 将 ADK Agent 连接到**文件系统 MCP 服务器**。Agent 可以通过模型上下文协议（MCP）执行读取、写入和列出文件等操作。

## 🎯 本示例展示的内容

- **MCP 服务器连接**：连接 `@modelcontextprotocol/server-filesystem`
- **文件操作**：读取、写入以及列出文件和目录
- **Stdio 通信**：通过标准输入/输出与本地 MCP 服务器通信
- **自动工具发现**：让 ADK 自动发现并使用可用的文件系统工具

## 🔧 工作原理

### MCP 服务器配置
Agent 会连接到提供以下工具的文件系统 MCP 服务器：
- `list_directory`：列出文件和文件夹
- `read_file`：读取文件内容
- `write_file`：向文件写入内容
- `create_directory`：创建新目录

### 连接流程
```python
MCPToolset(
    connection_params=StdioServerParameters(
        command='npx',
        args=['-y', '@modelcontextprotocol/server-filesystem', '/path/to/folder']
    )
)
```

## 🚀 快速开始

### 前置条件
1. **Node.js 和 npm**：文件系统 MCP 服务器运行所必需
2. **ADK 环境**：已配置 Google AI API Key
3. **依赖**：已通过 `requirements.txt` 安装所需包

### 运行 Agent

1. **进入教程根目录**：
   ```bash
   cd ai_agent_framework_crash_course/google_adk_tutorials/
   ```

2. **启动 ADK Web**：
   ```bash
   adk web
   ```

3. **选择 Agent**：从下拉菜单中选择 `filesystem_mcp_agent`

4. **测试命令**：
   - “列出当前目录中的文件”
   - “读取 sample.txt 的内容”
   - “创建名为 hello.txt 的新文件，内容为 Hello World!”
   - “显示目录中的所有文本文件”

## 📝 对话示例

### 列出文件
```text
用户：列出当前目录中的文件
Agent：我来为你列出当前目录中的文件。
[调用 list_directory 工具]
找到以下文件：
- sample.txt
- hello.txt
```

### 读取文件
```text
用户：读取 sample.txt 的内容
Agent：我来读取 sample.txt。
[调用 read_file 工具]
文件内容：
这是用于演示 MCP 文件系统 Agent 的示例文件。
你可以通过 MCP 工具读取、写入和列出文件。
```

### 写入文件
```text
用户：创建一个名为 notes.txt 的文件，内容为“今天的会议记录”
Agent：我将使用你指定的内容创建 notes.txt。
[调用 write_file 工具]
已成功创建 notes.txt，并写入“今天的会议记录”。
```

## 🔧 代码结构

### Agent 定义
```python
root_agent = LlmAgent(
    model='gemini-3-flash-preview',
    name='filesystem_mcp_agent',
    instruction="""
    You are a helpful filesystem assistant that can help users manage their files.
    You have access to filesystem tools through the Model Context Protocol (MCP).
    """,
    tools=[
        MCPToolset(
            connection_params=StdioServerParameters(
                command='npx',
                args=['-y', '@modelcontextprotocol/server-filesystem', DEMO_FOLDER]
            )
        )
    ]
)
```

### 演示环境
Agent 使用其文件所在目录的父目录作为演示范围：
- **位置**：`filesystem_agent` 文件夹的父目录
- **示例文件**：包含演示内容的 `sample.txt`
- **工作目录**：MCP 服务器可以访问该目录并在限定范围内安全操作

## 🛠️ 可用工具

文件系统 MCP 服务器会自动提供以下工具：

| 工具 | 说明 | 参数 |
|---|---|---|
| `list_directory` | 列出文件和文件夹 | `path`（可选） |
| `read_file` | 读取文件内容 | `path`（必需） |
| `write_file` | 向文件写入内容 | `path`、`content` |
| `create_directory` | 创建新目录 | `path` |

## 🔍 高级用法

### 工具过滤
```python
MCPToolset(
    connection_params=StdioServerParameters(
        command='npx',
        args=['-y', '@modelcontextprotocol/server-filesystem', DEMO_FOLDER]
    ),
    tool_filter=['list_directory', 'read_file']  # 仅暴露指定工具
)
```

## 🚨 重要说明

- **安全性**：MCP 服务器只能访问指定目录
- **需要 Node.js**：文件系统服务器通过 `npx` 运行
- **工作目录**：示例使用父目录，方便访问项目文件
- **错误处理**：Agent 能够处理文件不存在和权限不足等错误

## 🔍 故障排查

### 常见问题

1. **找不到 Node.js**：
   ```bash
   # 安装 Node.js
   # macOS: brew install node
   # Ubuntu: sudo apt install nodejs npm
   ```

2. **权限错误**：
   - 确保目标目录可写
   - 检查文件权限

3. **MCP 服务器无法启动**：
   - 验证 Node.js 是否正确安装
   - 检查所需端口是否可用
   - 查看控制台日志

### 调试命令
```bash
# 直接测试 MCP 服务器
npx @modelcontextprotocol/server-filesystem /path/to/folder

# 使用调试日志运行
adk web --debug
```

## 🔗 后续步骤

完成本示例后，可以继续：
1. **自定义目录**：将 `DEMO_FOLDER` 修改为所需位置
2. **添加更多工具**：探索其他 MCP 服务器
3. **尝试服务器 Agent**：学习创建自定义 MCP 服务器
4. **集成工作流**：与其他 ADK 功能组合使用

## 📚 相关文档

- **[ADK MCP 工具](https://google.github.io/adk-docs/tools/mcp-tools/)**：官方文档
- **[MCP 文件系统服务器](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)**：服务器说明
- **[模型上下文协议](https://modelcontextprotocol.io/)**：协议规范
