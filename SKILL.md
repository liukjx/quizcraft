---
name: quizcraft
description: Generate multiple-choice quizzes from course documents and subtitle transcripts, then practice through retrieval-based learning.
---

# QuizCraft

**从课程资料/字幕文稿生成选择题，通过做题来巩固知识。**

这个 skill 的核心是 **retrieval practice（检索练习）**—— 从你的学习材料中提取知识点，生成选择题，让你通过主动回忆来加深记忆，而不是被动阅读。

## 工作区结构

```
quizcraft/
├── SKILL.md              # 本 skill 定义
├── MISSION.md            # 你的学习目标（为什么学这个）
├── RESOURCES.md           # 参考资料清单（课程链接、文档、视频）
├── NOTES.md               # 笔记/偏好记录
│
├── sources/               # 原始课程资料（字幕、文稿、笔记）
│   └── *.md / *.txt / *.html
│
├── quizzes/               # 生成的题库
│   ├── 0001-<dash-case>.md
│   └── 0002-<dash-case>.md
│
├── learning-records/      # 学习记录（做题记录、掌握程度）
│   └── 0001-<dash-case>.md
│
├── reference/             # 参考卡片 / 知识卡片
│   └── *.md
│
└── assets/                # 可复用组件（样式、模板）
```

## 工作流程

### 第一步：导入资料

将你的课程文档、字幕文稿放入 `sources/` 目录。

支持的格式：
- 纯文本（TXT、MD）
- HTML 文章
- 字幕文件（SRT、VTT）

### 第二步：生成选择题

用户请求 → AI 从 `sources/` 中选取一份资料 → 基于内容生成选择题。

**题型策略：**

| 题型 | 用途 | 占比 |
|------|------|------|
| 单选题 | 概念理解、定义记忆 | 60% |
| 多选题 | 综合判断、多项要点 | 20% |
| 判断题 | 易混淆点辨析 | 10% |
| 填空题 | 关键术语记忆 | 10% |

**出题原则：**

1. **选项长度一致** — 选项字数、字符数尽量接近，不给猜测线索
2. **干扰项有迷惑性** — 来自资料中的真实内容而非随意编造
3. **难度递进** — 基础题→理解题→综合题
4. **每题都有详细解析** — 解释为什么对、为什么错
5. **标注知识点来源** — 引用 `sources/` 中的原始内容，方便查证

### 第三步：做题练习

用户选择某份题库 → 逐题作答：
- ✅ 答对 → 进入下一题
- ❌ 答错 → AI 给出解析，然后可以换一个类似的题巩固，或直接下一题

### 第四步：记录与复习

- 每次练习结果写入 `learning-records/`
- AI 根据错题分析薄弱知识点
- 下次练习优先出薄弱领域的题

## 出题格式示例

```markdown
# 第 1 题（单选题）
在 Flutter 中，哪种 Widget 用于处理手势？

A. Container
B. GestureDetector
C. Column
D. Stack

**正确答案：** B

**解析：**
GestureDetector 是 Flutter 中处理触摸手势的无界面 Widget。
A ❌ Container 是布局 Widget，不处理手势。
C ❌ Column 是垂直布局 Widget。
D ❌ Stack 是层叠布局 Widget。

**知识点来源：**
sources/flutter-intro.md → "手势处理"
```