## 💲 支持联网访问的 AI 金融智能体团队

这个脚本演示了如何仅用约 20 行 Python 代码，基于 GPT-4o 构建一支协同工作的 AI 智能体团队，并让其充当金融分析师。该系统将网络搜索能力与金融数据分析工具相结合，从而提供全面的金融洞察。

### 功能特性
- 具有专业分工的多智能体系统：
    - Web 智能体：负责通用互联网研究
    - 金融智能体：负责详细的金融分析
    - 团队智能体：负责协调各智能体之间的协作
- 通过 YFinance 获取实时金融数据
- 使用 DuckDuckGo 进行网络搜索
- 使用 SQLite 持久化保存智能体交互记录

### 如何开始？

1. 克隆 GitHub 仓库
```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/multi_agent_apps/agent_teams/ai_finance_agent_team
```

2. 安装所需依赖：

```bash
pip install -r requirements.txt
```

3. 获取 OpenAI API Key

- 注册一个 [OpenAI 账户](https://platform.openai.com/)（也可以使用你选择的其他 LLM 提供商）并获取 API 密钥。
- 将 OpenAI API 密钥设置为环境变量：
```bash
export OPENAI_API_KEY='your-api-key-here'
```

4. 运行 AI 智能体团队
```bash
python3 finance_agent_team.py
```

5. 打开浏览器并访问控制台输出中提供的 URL，即可通过 Playground 界面与 AI 智能体团队进行交互。
