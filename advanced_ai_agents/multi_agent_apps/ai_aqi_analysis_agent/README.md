# 🌍 AQI 空气质量分析 Agent

### 🎓 免费分步教程
**👉 [点击这里查看完整分步教程](https://www.theunwindai.com/p/build-an-aqi-analysis-agent)，从零开始构建本项目，并了解详细代码讲解、原理说明和最佳实践。**

AQI Analysis Agent 是一个基于 Firecrawl 与 Agno AI Agent 框架构建的空气质量监测和健康建议工具。它通过分析实时空气质量数据并生成个性化健康建议，帮助用户更合理地安排户外活动。

## 功能特性

- **多 Agent 系统**
  - **AQI Analyzer**：获取并处理实时空气质量数据
  - **Health Recommendation Agent**：生成个性化健康建议

- **空气质量指标**
  - 综合空气质量指数（AQI）
  - 颗粒物（PM2.5 和 PM10）
  - 一氧化碳（CO）浓度
  - 温度
  - 湿度
  - 风速

- **综合分析**
  - 实时数据可视化
  - 健康影响评估
  - 活动安全性建议
  - 推荐适合户外活动的时间段
  - 分析天气条件与空气质量之间的关联

- **交互功能**
  - 基于位置进行分析
  - 考虑用户已有健康状况
  - 针对具体活动给出建议
  - 支持下载分析报告
  - 提供示例查询用于快速测试

## 运行方式

按照以下步骤配置并运行应用：

1. **克隆仓库**

```bash
git clone https://github.com/Shubhamsaboo/awesome-llm-apps.git
cd advanced_ai_agents/multi_agent_apps/ai_aqi_analysis_agent
```

2. **安装依赖**

```bash
pip install -r requirements.txt
```

3. **配置 API Keys**

- 从 https://platform.openai.com/api-keys 获取 OpenAI API Key
- 从 [Firecrawl](https://www.firecrawl.dev/app/api-keys) 获取 Firecrawl API Key

4. **运行 Gradio 应用**

```bash
python ai_aqi_analysis_agent_gradio.py
```

5. **访问 Web 界面**

终端会显示两个 URL：
- 本地地址：`http://127.0.0.1:7860`
- 临时公网地址：`https://xxx-xxx-xxx.gradio.live`

在浏览器中打开任意一个地址即可使用应用。

## 使用方式

1. 在 API Configuration 区域填写 API Keys。
2. 输入位置信息：
   - 城市名称
   - 州/省（部分地区可选）
   - 国家
3. 输入个人信息：
   - 健康状况（可选）
   - 计划进行的户外活动
4. 点击 `Analyze & Get Recommendations`，即可获得：
   - 当前空气质量数据
   - 健康影响分析
   - 活动安全建议
5. 也可以直接使用示例查询进行快速测试。

## 注意事项

空气质量数据通过 Firecrawl 的网页抓取能力获取。受缓存和请求频率限制影响，显示的数据不一定始终与源网站的实时数值完全一致。若需要最准确的实时数据，请同时参考原始数据来源网站。
