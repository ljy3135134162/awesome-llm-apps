# OpenAI Remote MCP Tool Bridge

学习如何在**不使用 Agent 框架**的情况下，将一个普通的 OpenAI Function Calling 循环连接到托管的 Streamable HTTP MCP Server。

这个脚本会完整展示整个桥接过程：

1. 打开远程 Streamable HTTP 连接。
2. 通过分页调用 `list_tools()` 发现全部 MCP 工具。
3. 将 MCP 的工具名称、描述和输入 Schema 转换为 OpenAI Function Tools。
4. 让模型决定是否需要调用工具。
5. 通过当前 MCP Session 分发工具请求。
6. 为对应工具调用返回经过边界控制、适合模型上下文的结果。
7. 重复以上流程，直到模型给出最终回答，或达到工具调用预算上限。

## 配置

使用 Python 3.11 或更高版本：

```bash
pip install -r requirements.txt
export OPENAI_API_KEY="your-openai-api-key"
```

默认服务器为 Parallel Search MCP：`https://search.parallel.ai/mcp`。这是一个可直接运行的托管端点，提供只读工具，并且不要求 MCP 身份验证。

## 不使用 OpenAI 发现工具

`--list-tools` 会连接服务器并打印实时可用的工具名称与描述。该模式不需要 `OPENAI_API_KEY`。

```bash
env -u OPENAI_API_KEY python openai_remote_mcp_bridge.py --list-tools
```

## 运行桥接器

建议使用需要最新信息的任务，让模型自然地产生调用远程工具的需求：

```bash
python openai_remote_mcp_bridge.py \
  "Read https://news.ycombinator.com/ now. What is the title and destination URL of the number-one story?"
```

执行日志会显示 `[connect]`、`[discover]`、`[convert]`、每一次 OpenAI → MCP 请求、每一次 MCP → OpenAI 返回结果，以及最终的 `[final]`。

默认情况下，模型会获得可信服务器上发现的所有兼容工具。如果只想暴露其中一部分，可以先通过 `--list-tools` 获取名称，然后指定：

```bash
python openai_remote_mcp_bridge.py \
  --tools tool_name_one,tool_name_two \
  "Complete a task that needs those tools."
```

也可以修改模型、服务器地址或总工具调用预算：

```bash
python openai_remote_mcp_bridge.py \
  --server-url https://your-trusted-server.example/mcp \
  --model gpt-4o-mini \
  --max-tool-calls 3 \
  "Complete a task with current information."
```

`MCP_SERVER_URL` 和 `OPENAI_MODEL` 可以分别作为上述参数的环境变量默认值。

## 转换逻辑

对于每个暴露给模型的 MCP 工具，桥接器会进行以下映射：

```text
MCP tool.name        -> OpenAI function.name
MCP tool.description -> OpenAI function.description
MCP tool.inputSchema -> OpenAI function.parameters
```

OpenAI Function 名称必须由 1–64 个字母、数字、下划线或连字符组成。MCP 的输入 Schema 必须描述一个顶层 JSON Object。如果某个工具不兼容，桥接器会直接报出该工具名称并提前失败，而不是静默修改其接口契约。

工具结果会优先使用 MCP 的 `structuredContent`。如果没有，则会拼接文本块；图片、音频及其他非文本内容会被替换为简短标记。过大的输出会在进入模型上下文之前被截断。

每一个请求的工具调用都会计入 `--max-tool-calls`，包括参数格式错误、未知工具、调用失败以及被跳过的请求。每个请求都会获得一个对应的工具响应；当预算耗尽时，模型还会获得最后一次禁止继续调用工具的回答机会。

## 范围与安全

本教程刻意限定为：**一个可信、无需认证、只读的 Streamable HTTP MCP Server**。在将某个服务器的工具暴露给模型之前，应先使用 `--list-tools` 检查其内容。

生产环境中的 OAuth、写操作审批、多服务器路由、MCP Resources 与 Prompts，以及通用 JSON Schema 转换等问题都不在本示例范围内。实际应用中应自行加入这些策略，不要把 `--tools` 当作真正的授权边界。

## 故障排查

- `OPENAI_API_KEY is required`：设置 API Key，或者使用 `--list-tools` 在不调用 OpenAI 的情况下测试 MCP。
- `Unknown MCP tool`：针对相同的 `--server-url` 运行 `--list-tools`，然后更新 `--tools`。
- `not a valid OpenAI function name` 或 `must have an object input schema`：所选服务器暴露的某个工具无法直接表示为 OpenAI Function Tool。
- `Bridge failed`：确认服务器使用 Streamable HTTP、无需 MCP 身份验证即可访问，并且预期用途确实是只读。
