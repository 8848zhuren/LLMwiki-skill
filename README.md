# LLM Wiki Skill

> 让大模型成为你的个人知识管家。 基于 MiniMax M2.7 构建，快速部署，长期复利。

[![Stars](https://img.shields.io/github/stars/YOUR_USERNAME/LLMwiki-skill?style=social)](https://github.com/YOUR_USERNAME/LLMwiki-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 背景：为什么需要 LLM Wiki？

传统的知识管理，要么是"文件夹 + 关键词搜索"，要么是 RAG"上传文件 + 向量检索"。这些方案的共同问题是：**每次查询都是独立的，知识和知识之间没有连接，知识不会随着使用而变聪明**。

**LLM Wiki** 的思路来自 Andrej Karpathy，是一种完全不同的知识管理范式：

> 不是让 AI 从文档里找答案，而是让 AI **提前把每份资料编译成一张知识网**。当新问题到来时，AI 在这张网上推理，综合多份资料给出深度回答。

**核心差异**：

| | 传统 RAG | LLM Wiki |
|---|---|---|
| 知识组织 | 散落文档 | 结构化知识网络 |
| 查询方式 | 片段检索 | 跨页面综合推理 |
| 知识增长 | 线性堆积 | 复利效应，越积累越聪明 |
| 答案质量 | 片段拼凑 | 多资料深度综合 |

---

## 为什么选择 MiniMax M2.7？

LLM Wiki 的核心环节有两个：**Ingest（编译入库）** 和 **Query（查询回答）**。两者都对模型有极高要求。

**MiniMax M2.7 是目前最契合这个场景的选择：**

### 1. 超大上下文窗口，支撑跨页面推理

LLM Wiki 的查询往往需要同时读取 5-10 个相关页面，综合分析后才能给出完整答案。MiniMax M2.7 的 200K 上下文窗口可以一次加载所有相关页面，不需要反复召回和拼接，推理过程更连贯、答案质量更高。

### 2. 强大的中文理解与生成能力

知识库的原始资料大量来自中文文章、报告、笔记。MiniMax M2.7 对中文长文本的摘要、提炼、跨文档关联能力出色，能够准确理解资料的核心观点，而不是停留在关键词匹配层面。

### 3. 出色的多跳推理能力

LLM Wiki 的价值在于发现"表面不相关但实质有联系"的内容。比如用户问"最近有哪些 AI 创业公司获得了大额融资，它们和哪些技术趋势相关"，这需要模型在多个实体页面和概念页面之间跳跃推理。MiniMax M2.7 在这类多跳推理任务上表现稳定。

### 4. 成本效益

相比 GPT-4o 和 Claude 3.5，MiniMax M2.7 在保持同等推理质量的前提下，成本更具竞争力。个人用户和小型团队可以长期稳定使用，而不用担心知识库维护的成本压力。

---

## 核心特性

- **一键部署**：运行 SKILL.md，AI 自动完成目录搭建、Git 初始化、Schema 配置
- **AI 自动编译**：新资料入库后，MiniMax M2.7 自动生成摘要、提取实体和概念、建立双向链接
- **自然语言查询**：直接问知识库问题，AI 读取相关页面后综合回答，并标注来源
- **版本控制**：每次变更自动 Git 提交，可随时回滚
- **Obsidian 友好**：输出标准 Markdown，可直接用 Obsidian 浏览、图谱、查询

---

## 目录结构

```
LLMwiki-skill/
├── README.md              # 本文件
├── SKILL.md               # AI Agent 部署指南（AI 读取此文件引导用户部署）
├── schema/
│   ├── SCHEMA.md          # Wiki 结构定义、页面类型、命名规范
│   └── PROMPTS.md         # Ingest 和 Query 的提示词模板
└── .gitignore
```

---

## 快速部署

### 方式一：AI 引导式（推荐）

1. 把本仓库克隆到本地
2. 把 `SKILL.md` 内容告诉你的 AI 助手（瑶儿 / Claude Code / OpenCat 等）
3. 对 AI 说：`帮我部署 LLM Wiki`
4. AI 读取 SKILL.md，自动完成所有配置

### 方式二：手动部署

```bash
# 克隆仓库
git clone https://github.com/YOUR_USERNAME/LLMwiki-skill.git ~/llm-wiki
cd ~/llm-wiki

# 创建目录结构
mkdir -p raw/{articles,papers,books,notes,docs}
mkdir -p wiki/{summaries,entities/{people,companies,products},concepts/{work,tech,life,invest,research},comparisons,overviews,logs}
mkdir -p schema inbox

# 初始化 Git
git init

# 把 Obsidian 指向 wiki 目录，开始使用
```

---

## 工作原理

### Ingest（资料入库）

```
raw/ 中放入新资料（文章/文档/笔记）
        ↓
MiniMax M2.7 读取原文件
        ↓
MiniMax M2.7 执行：
  ├── 生成 summary 页（这份资料的摘要）
  ├── 提取 entities（人物/公司/产品 → 更新或新建 entity 页）
  ├── 提取 concepts（方法论/技术/趋势 → 更新或新建 concept 页）
  ├── 建立双向交叉引用链接
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
答案 + 标注来源（用户可回查原文）
```

---

## 配套工具

### Obsidian（浏览界面）

推荐安装插件：

| 插件 | 用途 |
|------|------|
| Git | 版本控制同步 |
| Dataview | 高级查询（按标签/日期/类型筛选）|
| Excalidraw | 知识图谱可视化 |
| Calendar | 按时间线浏览 |

### 提示词模板

`schema/PROMPTS.md` 中包含了：

- **Ingest Prompt**：指导 MiniMax M2.7 如何处理一份新资料
- **Query Prompt**：指导 MiniMax M2.7 如何回答用户问题
- **Health Check Prompt**：定期检查 Wiki 健康度

---

## 适用场景

- 个人知识积累（阅读笔记、行业研究、技术学习）
- 工作知识库（营销方案、客户资料、行业报告）
- 项目文档管理（多方资料汇总、决策记录）
- 投资研究（项目跟踪、竞品分析、市场调研）

---

## 局限性

- 每份资料入库需要 AI 处理，前期投入比直接 RAG 上传大
- 知识库质量依赖 AI 整理质量，需定期检查
- 不适合实时性内容（股价、新闻等）
- Wiki 超过 500 页面后，建议引入向量检索增强

---

## License

MIT © 2026

---

## 关于 MiniMax M2.7

本项目全程基于 MiniMax M2.7 开发完成。超长上下文窗口 + 出色的中文推理能力 + 合理的成本，使它成为个人 LLM Wiki 场景的最优选择。

[申请 MiniMax API](https://platform.minimaxi.com/)
