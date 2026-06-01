# AI 智能搜题

> 一款面向小学生的 **AI 搜题陪练 Agent**，核心理念不是"给答案"，而是"引导思考"。通过多模态图片识别、分步解析、思考模式引导、错题本沉淀与举一反三练习，帮助学生真正理解每一道题。

---

## 项目简介

传统搜题工具让学生"只看答案、不动脑筋"——82% 的学生搜题后直接点击查看全解，平均停留仅 12 秒，举一反三正确率仅 35%。

**AI 智能搜题** 通过以下方式解决这一痛点：

- **拍照识题**：多模态 LLM 直接识别图片中的题目文字
- **智能解析**：自动识别科目、题型、知识点，生成分步解题方案
- **思考模式**：AI 不直接讲完，而是围绕学生卡点逐步追问、针对性讲解
- **可视化讲解**：根据题型自动匹配线段图、数轴图等可视化模板
- **错题本**：自动沉淀对话摘要，提取知识薄弱点
- **举一反三**：基于错题和薄弱点生成针对性练习题

当前 MVP 聚焦 **小学数学** 高频题型。

---

## 项目结构

```
AIsouti/
├── 产品原型/                          # 核心产品原型（前后端完整实现）
│   ├── src/react_agent/               # 后端 AI Agent（基于 LangGraph）
│   │   ├── graph.py                   # LangGraph 状态图定义
│   │   ├── tools.py                   # Agent 工具集（OCR、解析、解题、讲解等）
│   │   ├── prompts.py                 # 系统提示词
│   │   ├── context.py                 # Agent 运行时上下文
│   │   ├── state.py                   # Agent 状态模型
│   │   ├── schemas.py                 # Pydantic 数据结构定义
│   │   ├── models.py                  # SQLAlchemy 数据模型
│   │   ├── database.py                # SQLite 数据库配置
│   │   ├── crud.py                    # 数据访问层
│   │   ├── server.py                  # FastAPI 服务接口
│   │   └── utils.py                   # 工具函数
│   ├── frontend/                      # 前端页面（HTML/CSS/JS）
│   │   ├── index.html                 # 搜一搜首页
│   │   ├── result.html                # 搜题结果页
│   │   ├── thinking.html              # 思考模式页
│   │   ├── errorbook.html             # 错题本页
│   │   ├── practice.html              # 举一反三练习页
│   │   ├── analysis.html              # 学习分析页
│   │   ├── conversation.html          # 对话记录页
│   │   ├── styles.css                 # 全局样式
│   │   └── js/                        # 各页面交互逻辑
│   ├── tests/                         # 测试用例
│   │   ├── unit_tests/                # 单元测试
│   │   └── integration_tests/         # 集成测试
│   ├── pyproject.toml                 # Python 项目配置
│   ├── langgraph.json                 # LangGraph 配置
│   └── Makefile                       # 常用命令
│
├── 产品分析与结构/                      # 产品文档与分析
│   ├── 产品页面结构.md                  # 前后端页面功能描述与交互设计
│   ├── 智能体构建逻辑.md                # Agent 二开技术方案与业务逻辑设计
│   ├── EARS需求文档_解题功能.md         # 核心需求（EARS 格式）
│   ├── SWOT竞品分析_小猿系产品.md       # 竞品 SWOT 分析
│   └── UI界面需求解释（持续维护）文档.md  # 原型页面需求分析
│
├── prototype/                           # 硬编码 UI 原型（纯静态展示）
│   ├── index.html                       # 搜一搜首页
│   ├── result.html                      # 搜题结果页
│   ├── thinking.html                    # 思考模式页
│   ├── errorbook.html                   # 错题本页
│   ├── practice.html                    # 举一反三练习页
│   ├── analysis.html                    # 学习分析页
│   └── styles.css                       # 全局样式
│
└── Github baseline与逆向PRD分析/        # 基线项目分析
    ├── 全能搜题网页端/                   # 开源基线项目（React 前端）
    └── 全能搜题网页端-PRD.md             # 基线项目逆向 PRD
```

### UI 原型说明（prototype/）

`prototype/` 目录存放的是一套**硬编码的静态 UI 原型**，用于早期产品视觉验证和交互走查。

- **纯前端静态页面**：使用 HTML + CSS 硬编码所有页面内容与布局，不含任何动态数据绑定或后端交互逻辑。
- **数据为 Mock 写死**：页面中的题目、答案、对话记录、统计数据等均为写死的示例数据，不代表真实业务状态。
- **用途**：供产品评审、设计参考和研发理解页面结构，不作为最终交付物。
- **与产品原型的区别**：`产品原型/frontend/` 是具备 JS 交互逻辑、可对接后端 API 的功能原型；`prototype/` 仅用于视觉和布局层面的快速验证。

如需预览，可通过任意静态服务器启动：

```bash
cd prototype
python -m http.server 8080
```

然后访问 `http://localhost:8080`。

---

## 技术栈

### 后端

| 技术 | 用途 |
|---|---|
| [LangGraph](https://github.com/langchain-ai/langgraph) | AI Agent 编排框架（ReAct 模式） |
| [LangChain](https://github.com/langchain-ai/langchain) | LLM 调用与工具集成 |
| [FastAPI](https://fastapi.tiangolo.com/) | Web API 服务 |
| [Uvicorn](https://www.uvicorn.org/) | ASGI 服务器 |
| [SQLAlchemy](https://www.sqlalchemy.org/) | ORM 数据访问 |
| [SQLite](https://www.sqlite.org/) | 本地数据持久化 |
| [Pydantic](https://docs.pydantic.dev/) | 数据验证与结构化输出解析 |
| Python >= 3.11 | 运行环境 |

### 前端

| 技术 | 用途 |
|---|---|
| HTML5 + CSS3 + Vanilla JS | 原型页面实现 |
| [Lucide Icons](https://lucide.dev/) | 图标库 |

### LLM 支持

支持任意 **OpenAI-compatible Chat Completions API**，包括但不限于：

- OpenAI（GPT-4o 等）
- Anthropic（Claude 系列）
- 阿里 DashScope / Qwen 系列
- 其他兼容 OpenAI 格式的模型服务

多模态输入使用 `text` + `image_url` 标准格式。

---

## 快速开始

### 1. 环境准备

确保已安装 Python >= 3.11，推荐使用 [uv](https://docs.astral.sh/uv/) 管理依赖。

### 2. 安装依赖

```bash
cd 产品原型
uv sync
```

### 3. 配置环境变量

```bash
cp .env.example .env
```

编辑 `.env`，填入你的 LLM 配置：

```env
# 通用 OpenAI-compatible 配置（优先使用）
LLM_PROVIDER=openai_compatible
LLM_API_KEY=your-api-key
LLM_BASE_URL=your-base-url
LLM_MODEL=your-model-name

# 可选：DashScope / Qwen 配置
DASHSCOPE_API_KEY=
QWEN_BASE_URL=
QWEN_MODEL=

# 数据库（默认即可）
DATABASE_URL=sqlite:///./homework_agent.sqlite3
```

### 4. 启动服务

**后端 API：**

```bash
cd 产品原型
uv run uvicorn react_agent.server:app --reload --host 0.0.0.0 --port 8000
```

**前端页面：**

```bash
cd 产品原型/frontend
python -m http.server 8081
```

### 5. 访问

| 服务 | 地址 |
|---|---|
| 后端 API | `http://localhost:8000` |
| API 文档 | `http://localhost:8000/docs` |
| 健康检查 | `http://localhost:8000/health` |
| 前端页面 | `http://localhost:8081` |

---

## API 接口

| 方法 | 路径 | 说明 |
|---|---|---|
| `GET` | `/health` | 健康检查 |
| `POST` | `/api/homework/submit` | 提交题目（支持文本、图片 URL、base64） |
| `POST` | `/api/homework/solution` | 获取解题方案 |
| `POST` | `/api/chat/general` | 通用对话（自由提问） |
| `POST` | `/api/chat/general/stream` | 通用对话（SSE 流式输出） |
| `POST` | `/api/homework/wrongbook/save` | 保存题目到错题本 |
| `POST` | `/api/homework/practice/generate` | 生成举一反三练习题 |

---

## 核心业务流程

```
学生上传题目图片 / 输入题目文本
        ↓
   多模态 LLM OCR 识别题目
        ↓
   智能解析（科目、题型、知识点、难度）
        ↓
   生成标准答案 + 分步解题步骤
        ↓
   询问：进入思考模式？
    ┌──────┴──────┐
    ↓ 是           ↓ 否
  逐步引导思考    直接展示答案
  围绕卡点讲解    （保留切换入口）
  可视化辅助
  循环直到理解
        ↓
   学生尝试作答 → 校验答案
        ↓
   询问：加入错题本？
    ┌──────┴──────┐
    ↓ 是           ↓ 否
  保存题目       不记录
  生成对话摘要
  提取薄弱点
        ↓
   举一反三：基于错题生成 3 道练习题
```

---

## 测试

```bash
cd 产品原型

# 单元测试
make test

# 集成测试
make integration_tests

# 代码检查
make lint

# 格式化
make format
```

---

## 产品文档索引

| 文档 | 说明 |
|---|---|
| [产品页面结构](产品分析与结构/产品页面结构.md) | 前后端页面功能、交互设计与业务流程完整描述 |
| [智能体构建逻辑](产品分析与结构/智能体构建逻辑.md) | Agent 二次开发技术方案、State 设计、数据库设计与 API 设计 |
| [EARS 需求文档](产品分析与结构/EARS需求文档_解题功能.md) | 三大核心需求（引导式提问、可视化讲解、错题沉淀）|
| [SWOT 竞品分析](产品分析与结构/SWOT竞品分析_小猿系产品.md) | 与小猿系产品的全面竞品分析 |
| [UI 需求解释](产品分析与结构/UI界面需求解释（持续维护）文档.md) | 原型页面截图与需求分析对照 |
| [基线项目 PRD](Github%20baseline与逆向PRD分析/全能搜题网页端-PRD.md) | 全能搜题开源基线项目逆向 PRD |

---

## 基线项目

本项目后端基于 [langchain-ai/react-agent](https://github.com/langchain-ai/react-agent) 进行二次开发，保留了 LangGraph ReAct Agent 的核心架构，在其基础上扩展了教育场景专用的工具集、数据持久化和 FastAPI 接口层。

前端参考了 [全能搜题网页端](https://github.com/zmide/study.zmide.com)（MIT License）的产品设计。

---

## License

MIT
