# LLM Wiki Skill — AI Agent 部署指南

> 本文件供 AI Agent 读取。当用户说"帮我部署 LLM Wiki"时，AI 读取本文件并按步骤执行。
> 核心目标：AI 独立完成全流程，用户只需确认关键决策。

---

## 前置检查

在开始之前，AI 需要确认以下事项：

### 1. 确认用户意图

用户是否已有本地存储目录？是否已有 GitHub 账号和 Token？

如果用户没有 GitHub Token，告诉用户：
> "请先去 GitHub Settings → Developer settings → Personal access tokens 生成一个 Token，勾选 `repo` 权限，然后把 Token 发给我。"

### 2. 确定存储路径

询问用户想把 Wiki 存在哪里：

```
请选择 Wiki 存放路径：
A. 默认位置：~/llm-wiki（Linux/macOS/WSL）
B. 自定义路径：请告诉我完整路径
```

---

## 一、创建目录结构

AI 自动执行（或用户确认路径后执行）：

```bash
# 用户确认路径后，替换 WIKI_ROOT 为实际路径
WIKI_ROOT=~/llm-wiki

mkdir -p $WIKI_ROOT/raw/{articles,papers,books,notes,docs}
mkdir -p $WIKI_ROOT/wiki/{summaries,entities/{people,companies,products},concepts/{work,tech,life,invest,research},comparisons,overviews,logs}
mkdir -p $WIKI_ROOT/schema
mkdir -p $WIKI_ROOT/inbox
touch $WIKI_ROOT/wiki/logs/.gitkeep

echo "目录结构创建完成"
```

---

## 二、初始化 Git 仓库

```bash
cd $WIKI_ROOT
git init
git add .
git commit -m "feat: initial LLM Wiki structure"
```

---

## 三、配置 GitHub 远程仓库

**需要用户提供以下信息：**

1. GitHub Personal Access Token（Token）
2. GitHub 用户名（Username）
3. 仓库名（默认：`llm-wiki`，可自定义）

收到信息后：

```bash
# 设置 remote（替换 YOUR_TOKEN 和 YOUR_USERNAME）
git remote add origin https://YOUR_TOKEN@github.com/YOUR_USERNAME/llm-wiki.git

# 推送初始提交
git branch -M main
git push -u origin main
```

---

## 四、复制 Skill 文件到 Wiki 根目录

本 Skill 仓库包含两个关键文件，需要复制到 Wiki 根目录：

| 文件 | 来源 | 复制到 |
|------|------|--------|
| `schema/SCHEMA.md` | 本仓库 `schema/SCHEMA.md` | `$WIKI_ROOT/schema/SCHEMA.md` |
| `schema/PROMPTS.md` | 本仓库 `schema/PROMPTS.md` | `$WIKI_ROOT/schema/PROMPTS.md` |

```bash
cp -r schema/ $WIKI_ROOT/
git add .
git commit -m "feat: add schema and prompts"
git push
```

---

## 五、创建 Obsidian 快捷方式（可选）

如果用户使用 Obsidian，提示用户：

> "请在 Obsidian 中打开文件夹 `$WIKI_ROOT/wiki`，这样可以直接浏览知识库。推荐安装 Git 插件，AI 每次更新 Wiki 后自动同步。"

---

## 六、验证部署

AI 执行以下验证：

```bash
echo "=== 目录结构验证 ==="
find $WIKI_ROOT -type d | grep -v ".git" | sort

echo ""
echo "=== Git 状态 ==="
cd $WIKI_ROOT && git log --oneline -3

echo ""
echo "=== 下一步 ==="
echo "1. Wiki 目录已创建并推送到 GitHub"
echo "2. 把第一份资料放入 raw/articles/ 然后告诉我：帮我处理 [文件名]"
echo "3. 我会读取资料，生成 Wiki 页面，建立链接"
```

---

## 七、完整流程总结

```
用户启动部署
        ↓
AI 询问：存储路径 + GitHub Token
        ↓
用户回复信息
        ↓
AI 执行：
  ├── 创建目录结构 ✓
  ├── 初始化 Git ✓
  ├── 推送初始 commit 到 GitHub ✓
  ├── 复制 schema 文件 ✓
  └── 验证完成 ✓
        ↓
AI 汇报：
  - "Wiki 已就绪，存放在 ~/llm-wiki"
  - "可以把第一份资料丢进 raw/articles/，然后告诉我"
```

---

## 用户引导话术模板

AI 在每个步骤使用对应话术：

**第一步（确认信息）：**
> "我来帮你搭建 LLM Wiki。先确认几个信息：
> 1. Wiki 存在哪里？（默认 `~/llm-wiki`）
> 2. 有 GitHub Token 吗？（需要 `repo` 权限）
> 3. GitHub 用户名是？"

**第二步（创建完成）：**
> "✅ 目录结构已创建，推送到了 GitHub。
> 现在你可以 Obsidian 打开 `~/llm-wiki/wiki`，或者直接把第一份资料丢进 `~/llm-wiki/raw/articles/`，告诉我'帮我处理这篇'，我来生成 Wiki 页面。"

**遇到错误：**
> "出了点问题：[错误描述] 请检查一下，或者重新给我 GitHub Token，我再试一次。"

---

## 附录：目录结构说明

```
~/llm-wiki/              ← Wiki 根目录
├── raw/                 ← 原始资料（你放文件的地方）
│   ├── articles/        ← 网络文章、博客
│   ├── papers/          ← 论文、学术报告
│   ├── books/           ← 书籍摘录
│   ├── notes/           ← 随手笔记
│   └── docs/            ← 工作文档、PPT、PDF
│
├── wiki/                ← AI 编译后的知识库（Obsidian 浏览这里）
│   ├── summaries/       ← 每份资料的摘要页
│   ├── entities/       ← 实体页（人物/公司/产品）
│   ├── concepts/       ← 概念页（方法论/技术/趋势）
│   ├── comparisons/    ← 对比分析页
│   ├── overviews/      ← 领域全景图
│   └── logs/           ← 操作日志
│
├── schema/              ← 治理规则（别动）
│   ├── SCHEMA.md       ← Wiki 结构规范
│   └── PROMPTS.md      ← AI 提示词模板
│
└── inbox/              ← 临时入口（碎片随手丢）
```

---

*本 SKILL.md 由 AI Agent 读取使用，帮助用户快速部署 LLM Wiki。*
