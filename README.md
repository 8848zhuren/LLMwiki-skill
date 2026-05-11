# LLM Wiki Skill

> **极简 + AI Agent 原生**。不是又一个桌面应用，而是让任何 AI 助手都能引导用户搭建 LLM Wiki 的 Skill。5 分钟部署，零门槛。

[![Stars](https://img.shields.io/github/stars/8848zhuren/LLMwiki-skill?style=social)](https://github.com/8848zhuren/LLMwiki-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 背景：不一样的思路

nashsu/llm_wiki 是一个优秀的全功能桌面应用，功能完整、知识图谱、Deep Research——但它是一个**独立软件**，需要安装、配置、单独运行。

**LLM Wiki Skill** 的思路完全不同：

> 不做产品，做**技能**。发一个 SKILL.md 给 AI 助手，AI 引导用户完成所有部署工作。用户不需要安装任何东西，不需要懂技术，甚至不需要知道 LLM Wiki 是什么——只需要说一句"帮我部署"。

---

## 核心定位

| | 桌面应用方案（llm_wiki） | LLM Wiki Skill（我们） |
|---|---|---|
| **形态** | Electron 桌面应用 | AI Agent Skill 文件 |
| **部署** | 下载 → 安装 → 配置 | 告诉 AI"帮我部署" |
| **依赖** | 独立运行 | 任何 AI 助手都能用 |
| **维护** | 需要更新软件 | Skill 更新 AI 自动同步 |
| **门槛** | 需要安装桌面应用 | 零门槛，说人话就行 |

---

## 为什么选择 MiniMax M2.7？

LLM Wiki 的核心环节是 **Ingest（编译入库）** 和 **Query（查询回答）**，两者都需要模型具备出色的长文本理解、多文档推理能力。

**MiniMax M2.7 是这个场景的最优选择：**

### 1. 200K 上下文，支撑跨页面推理

一次 Ingest 往往需要同时读取多篇文档，综合分析后才能建立有效的交叉引用。MiniMax M2.7 的 200K 上下文窗口可以一次性加载所有相关页面，无需反复召回，推理过程更连贯，链接质量更高。

### 2. 出色的中文理解与生成能力

知识库的原始资料大量来自中文文章、报告、笔记。MiniMax M2.7 对中文长文本的摘要、提炼、跨文档关联能力优秀，能够准确理解资料的核心观点，建立真正有意义的链接。

### 3. 多跳推理能力

LLM Wiki 的价值在于发现"表面不相关但实质有联系"的内容——比如"这家 AI 创业公司的技术路线，和三年前某篇论文提出的方法有关联"。MiniMax M2.7 在这类跨页面多跳推理任务上表现稳定。

### 4. 成本与可持久性

相比 GPT-4o 和 Claude 3.5，MiniMax M2.7 在保持同等推理质量的前提下，成本更具竞争力。个人用户可以长期稳定使用知识库，而不用担心维护成本侵蚀收益。

---

## 核心特性

- **AI Agent 原生**：SKILL.md 被任何 AI 助手读取后，自动引导用户完成全流程部署
- **极简主义**：没有桌面应用、没有向量数据库、没有额外依赖，就是一个 Skill 文件
- **自然语言交互**：用户说"帮我部署"，AI 读取 SKILL.md → 自动完成目录搭建、Git 初始化、Schema 配置
- **Obsidian 友好**：输出标准 Markdown，可直接用 Obsidian 浏览、图谱、查询
- **版本控制**：每次变更自动 Git 提交，随时可回滚
- **MiniMax M2.7 驱动**：Ingest 和 Query 全程由 M2.7 提供推理能力

---

## 工作原理

### Ingest（资料入库）

```
用户把资料丢进 raw/ → 告诉 AI"帮我处理这篇"
        ↓
MiniMax M2.7 读取原文件
        ↓
MiniMax M2.7 执行：
  ├── 生成 summary 页
  ├── 提取 entities（人物/公司/产品）
  ├── 提取 concepts（方法论/技术/趋势）
  ├── 建立双向交叉引用
  └── 记录到 logs
        ↓
Git 自动 commit
```

### Query（知识查询）

```
用户问："最近看了哪些关于 AI 营销的资料？"
        ↓
MiniMax M2.7 搜索相关 Wiki 页面
        ↓
MiniMax M2.7 综合多页面信息，整理回答
        ↓
答案 + 标注来源
```

---

## 快速开始

### AI 引导式部署（推荐，5 分钟）

1. 把 `SKILL.md` 内容告诉你的 AI 助手
2. 对 AI 说：`帮我部署 LLM Wiki`
3. AI 读取 SKILL.md，引导你完成所有配置

### 手动部署

```bash
git clone https://github.com/8848zhuren/LLMwiki-skill.git ~/llm-wiki
cd ~/llm-wiki
# 把 Obsidian 指向 wiki 目录，开始使用
```

---

## 目录结构

```
llm-wiki/
├── README.md              # 本文件
├── SKILL.md               # AI Agent 部署指南（核心）
├── schema/
│   ├── SCHEMA.md          # Wiki 结构规范
│   └── PROMPTS.md         # Ingest/Query 提示词模板
└── .gitignore
```

---

## 适用场景

- **个人知识积累**：阅读笔记、行业研究、技术学习
- **工作知识库**：营销方案、客户资料、行业报告
- **AI 工作流集成**：作为 AI Agent 的内置技能，赋予长期记忆能力

---

## 与 llm_wiki 的关系

本项目受 [nashsu/llm_wiki](https://github.com/nashsu/llm_wiki) 启发。llm_wiki 是一个功能完整的桌面应用，适合需要图形界面和高级功能（知识图谱、Deep Research）的用户。

**我们选择了不同的路径**：不做应用，做技能。不追求功能丰富，追求**极简和普适**。任何一个 AI 助手，只要能读取 SKILL.md，就能引导用户部署完整的 LLM Wiki 系统。

---

## License

MIT © 2026

---

## 关于 MiniMax M2.7

本项目全程基于 MiniMax M2.7 开发完成。超长上下文窗口 + 优秀的中文推理能力 + 合理的成本，使它成为个人 LLM Wiki 场景的最优选择。

[申请 MiniMax API](https://platform.minimaxi.com/)
