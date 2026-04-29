# 🤖 AI Daily Spider

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Actions](https://github.com/GoodHappy666/ai-daily-spider/workflows/Daily%20Report/badge.svg)
![Last Commit](https://img.shields.io/github/last-commit/GoodHappy666/ai-daily-spider)

*AI 每日简报爬虫 - 自动抓取 AI/科技热点，生成每日简报*

[**在线演示**](https://brief-mind-frontend.vercel.app/) · [**后端 API**](https://ai-daily-backend.onrender.com/) · [**前端源码**](https://github.com/GoodHappy666/ai-daily-frontend)

</div>

---

## 📖 项目介绍

**AI Daily Spider** 是一个自动化的 AI 资讯聚合与简报生成系统。每天定时抓取多个主流 AI/科技媒体的最新资讯，通过 AI 智能分析提取关键信息，生成结构化的每日简报。

### ✨ 核心特性

- 🔄 **多源聚合** - 同时抓取多个 AI/科技媒体，确保资讯全面
- 🤖 **AI 驱动** - 利用大语言模型智能提取要点、生成摘要
- ⏰ **定时任务** - 通过 GitHub Actions 实现每日自动运行
- 📊 **结构化输出** - 生成易于阅读的简报格式
- 🚀 **零成本部署** - 完全使用免费服务，无服务器费用

---

## 🛠️ 技术栈

| 分类 | 技术 |
|------|------|
| **语言** | Python 3.10+ |
| **爬虫** | requests, BeautifulSoup4 |
| **AI 处理** | OpenAI GPT-4 / Claude |
| **定时任务** | GitHub Actions |
| **存储** | TiDB Serverless |
| **API** | Spring Boot + MyBatis-Plus |

---

## 🏗️ 系统架构

```
┌─────────────────────────────────────────────────────────────────┐
│                        GitHub Actions                            │
│                    (每日定时触发爬虫任务)                          │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      AI Daily Spider                            │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │  36氪    │  │  虎嗅    │  │  IT之家  │  │  更多...  │        │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘        │
│       │             │             │             │               │
│       └─────────────┴──────┬──────┴─────────────┘               │
│                            ▼                                     │
│                   ┌─────────────────┐                             │
│                   │   AI 处理器     │                             │
│                   │ (生成简报摘要)   │                             │
│                   └────────┬────────┘                             │
└────────────────────────────┼────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Spring Boot API                              │
│              (存储简报数据，提供查询接口)                          │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                   TiDB Serverless                               │
│                      (免费数据库)                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Vercel 前端                                    │
│            https://brief-mind-frontend.vercel.app               │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/GoodHappy666/ai-daily-spider.git
cd ai-daily-spider
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 配置环境变量

创建 `.env` 文件：

```bash
# OpenAI API（用于生成简报摘要）
OPENAI_API_KEY=sk-your-api-key

# 后端 API 地址（推送简报用）
API_BASE_URL=https://ai-daily-backend.onrender.com
API_TOKEN=your-api-token
```

### 4. 本地运行

```bash
# 抓取并生成简报
python main.py

# 仅测试爬虫（不调用 AI，不推送）
python main.py --dry-run
```

---

## 📋 数据源

当前已集成的资讯来源：

| 媒体 | 网址 | 分类 |
|------|------|------|
| 36氪 | 36kr.com | 科技/创业 |
| 虎嗅 | huxiu.com | 科技/商业 |
| IT之家 | ithome.com | 科技/数码 |
| 机器之心 | jiqizhixin.com | AI/机器学习 |
| AI前线 | ai.google | AI/研究 |

> 💡 如需添加新的数据源，参考 `sources/` 目录下的现有实现

---

## ⚙️ 配置说明

### GitHub Actions 定时任务

工作流文件位于 `.github/workflows/daily.yml`，默认每天 **北京时间 9:00** 自动运行：

```yaml
schedule:
  - cron: '0 1 * * *'  # UTC 1:00 = 北京时间 9:00
```

### 环境变量配置

| 变量名 | 必填 | 说明 |
|--------|------|------|
| `OPENAI_API_KEY` | ✅ | OpenAI API Key |
| `API_BASE_URL` | ✅ | 后端 API 地址 |
| `API_TOKEN` | ✅ | API 访问令牌 |
| `MAX_ARTICLES` | ❌ | 单次最大抓取文章数（默认 20） |

---

## 📁 项目结构

```
ai-daily-spider/
├── .github/
│   └── workflows/
│       └── daily.yml          # GitHub Actions 工作流
├── sources/                    # 爬虫模块
│   ├── __init__.py
│   ├── base.py                 # 基础爬虫类
│   ├── kr36.py                # 36氪爬虫
│   ├── huxiu.py               # 虎嗅爬虫
│   └── ithome.py              # IT之家爬虫
├── ai/                         # AI 处理模块
│   ├── __init__.py
│   └── summarizer.py          # 简报生成器
├── api/                        # API 交互模块
│   ├── __init__.py
│   └── client.py              # 后端 API 客户端
├── utils/
│   ├── __init__.py
│   └── config.py              # 配置管理
├── main.py                     # 程序入口
├── requirements.txt            # 依赖列表
└── README.md
```

---

## 🔧 常见问题

### Q: 爬虫运行失败怎么办？

1. 检查网络连接是否正常
2. 确认环境变量配置正确
3. 查看 GitHub Actions 日志定位问题

### Q: 如何本地调试？

```bash
# 查看详细日志
python main.py -v

# 测试单个数据源
python -m sources.kr36
```

### Q: API 请求超时？

Render 免费版有冷启动延迟，首次请求可能较慢，属于正常现象。

---

## 📜 许可证

本项目基于 [MIT License](LICENSE) 开源，欢迎 Star 和 Fork！

---

## 🙏 致谢

- [36氪](https://36kr.com/) - 优质科技资讯
- [虎嗅](https://www.huxiu.com/) - 深度商业报道
- [IT之家](https://www.ithome.com/) - 快速科技新闻
- [TiDB](https://tidbcloud.com/) - 免费云数据库
- [Render](https://render.com/) - 免费后端托管
- [Vercel](https://vercel.com/) - 免费前端托管

---

<div align="center">

⭐ 如果这个项目对你有帮助，欢迎点个 Star！

</div>
