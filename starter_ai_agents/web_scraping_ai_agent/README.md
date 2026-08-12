# 🕷️ Web Scraping AI Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-a-web-scraping-ai-agent-with-llama-3-2-running-locally)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

这是一个基于 **ScrapeGraphAI** 的 AI 网页抓取工具，可通过自然语言提示词从网站中提取结构化数据。该 Agent 使用开源 `scrapegraphai` 库，并可在本地运行。

---

## 📁 目录内容

**文件：** `ai_scrapper.py`、`local_ai_scrapper.py`

项目使用可在本地机器运行的开源 ScrapeGraphAI 库。

**✅ 优点：**
- 可免费使用，本地模型模式下可避免额外 API 成本
- 对执行过程拥有完整控制权
- 更有利于隐私保护，数据可保留在本地

**❌ 缺点：**
- 需要在本地安装运行环境和依赖
- 性能受本地硬件限制
- 需要自行管理依赖和版本更新

---

## 🚀 快速开始

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd awesome-llm-apps/starter_ai_agents/web_scraping_ai_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

3. **获取 OpenAI API Key**
- 注册 [OpenAI](https://platform.openai.com/) 账号
- 获取你的 API Key

4. **运行 Streamlit 应用**

```bash
streamlit run ai_scrapper.py
# 或使用本地模型：
streamlit run local_ai_scrapper.py
```

---

## 💡 使用场景

### 电商数据抓取

```python
# 提取商品信息
prompt = "Extract product names, prices, and availability"
```

### 内容聚合

```python
# 将文章转换为结构化数据
prompt = "Extract article title, author, date, and main content"
```

### 竞争情报

```python
# 监控竞争对手网站
prompt = "Extract pricing, features, and updates"
```

### 潜在客户信息收集

```python
# 提取联系信息
prompt = "Find company names, emails, and phone numbers"
```

---

## 🔧 工作原理

1. 提供 OpenAI API Key。
2. 选择模型，例如 GPT-4o、GPT-5 或本地模型。
3. 输入目标 URL 和数据提取提示词。
4. 应用使用 ScrapeGraphAI 抓取网页并在本地处理、提取数据。
5. 最终结果会显示在应用界面中。

---

## 📖 文档

- **ScrapeGraphAI 库：** [ScrapeGraphAI GitHub](https://github.com/VinciGit00/Scrapegraph-ai)

---
