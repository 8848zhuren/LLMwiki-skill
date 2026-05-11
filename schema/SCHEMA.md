# LLM Wiki Schema — 知识库结构规范

> 本文件定义 Wiki 的目录结构、页面类型、命名规范。所有 AI 编译和查询操作遵循本规范。

---

## 一、目录结构

```
raw/                    # 原始资料（不可变的事实来源）
├── articles/           #   网络文章、博客
├── papers/             #   论文、学术报告
├── books/              #   书籍摘录
├── notes/              #   随手记的碎片
└── docs/               #   工作文档、PPT、PDF

wiki/                   # AI 编译后的知识库
├── summaries/          #   每份原始资料的摘要页
├── entities/           #   实体页
│   ├── people/         #     人物
│   ├── companies/      #     公司
│   └── products/       #     产品
├── concepts/           #   概念页
│   ├── work/           #     工作类
│   ├── tech/           #     技术类
│   ├── life/           #     生活类
│   ├── invest/         #     投资类
│   └── research/       #     研究类
├── comparisons/        #   对比分析页（A vs B）
├── overviews/          #   领域全景图
└── logs/               #   操作日志

schema/                 # 治理规则
inbox/                  # 临时入口
```

---

## 二、页面类型

| 类型 | 存放位置 | 用途 |
|------|---------|------|
| Summary | `wiki/summaries/` | 某份原始资料的摘要 |
| Entity | `wiki/entities/{type}/` | 人/公司/产品/项目 |
| Concept | `wiki/concepts/{domain}/` | 方法论/技术/理论/趋势 |
| Comparison | `wiki/comparisons/` | 两个事物的对比分析 |
| Overview | `wiki/overviews/` | 某个领域的全景图 |

---

## 三、Slug 命名规范

- 全部小写
- 单词之间用连字符 `-` 连接
- 不含特殊字符
- 示例：`openai-gpt4` / `karpathy-llm-wiki` / `mini-max-m2-7`

---

## 四、YAML Front Matter 标准格式

```yaml
---
title: [页面标题]
slug: [url-friendly-slug]
tags: [entity/person, domain/tech, concept, comparison, overview]
domains: [tech, work]
sources: [source-slug-1, source-slug-2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
summary: [一句话概括核心内容，用于快速判断相关性]
---
```

---

## 五、正文结构标准

所有页面正文必须包含以下四个部分：

```markdown
# [页面标题]

## 核心要点
（AI 总结的关键内容，3-5 条）

## 详细信息
（详细展开，可以有子章节）

## 相关页面
- [[related-page-slug]] （相关 entity 或 concept）
- [[another-related]]

## 来源
- [原始资料名称](file://../../raw/articles/article-slug.md)
```

---

## 六、Domain 标签定义

| 标签 | 说明 | 示例 |
|------|------|------|
| `#domain/work` | 工作相关 | 营销方案、客户资料、工作流程 |
| `#domain/tech` | 技术相关 | AI、工具、代码 |
| `#domain/life` | 生活相关 | 健身、饮食 |
| `#domain/invest` | 投资相关 | 股票、加密货币 |
| `#domain/research` | 研究相关 | 竞品分析、市场调研 |

---

## 七、Entity 标签定义

| 标签 | 说明 | 示例 |
|------|------|------|
| `#entity/person` | 人物 | 创业者、投资人 |
| `#entity/company` | 公司 | 竞品、客户 |
| `#entity/product` | 产品 | 具体产品或服务 |
| `#entity/project` | 项目 | 正在跟踪的项目 |

---

## 八、版本控制规则

- 每次 Ingest 完成自动 Git commit
- Commit 格式：`feat: ingest [filename] → [pages_created]`
- 每次 Query 不产生 commit
- 定期 Health Check 后根据实际变更情况 commit

---

## 九、日志格式

```yaml
---
date: YYYY-MM-DD HH:MM
action: ingest / update / health_check
source: raw/articles/xxx.md
pages_created: [summaries/xxx, concepts/tech/yyy]
pages_updated: [entities/companies/zzz]
---
```
