---
name: first-time-setup
description: baoyu-translate 偏好首次设置流程
---

# 首次设置

## 概述

未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：必须先完成本设置，再执行任何翻译。禁止：
- 开始翻译内容
- 询问文件或输出路径
- 进入任何工作流步骤

仅按本流程提问，保存 EXTEND.md 后再继续。

## 设置流程

```
No EXTEND.md found
        |
        v
+---------------------+
| AskUserQuestion     |
| (all questions)     |
+---------------------+
        |
        v
+---------------------+
| Create EXTEND.md    |
+---------------------+
        |
        v
    Continue translation
```

## 问题

**语言**：使用用户输入语言或已保存的语言偏好。

在**一次** AskUserQuestion 调用中包含**全部**问题：

### 问题 1：目标语言

```yaml
header: "目标语言"
question: "默认目标语言？"
options:
  - label: "简体中文 zh-CN（推荐）"
    description: "译为简体中文"
  - label: "繁體中文 zh-TW"
    description: "译为繁体中文"
  - label: "English en"
    description: "译为英语"
  - label: "日本語 ja"
    description: "译为日语"
```

说明：用户也可输入自定义语言代码。

### 问题 2：翻译模式

```yaml
header: "模式"
question: "默认翻译模式？"
options:
  - label: "Normal（推荐）"
    description: "先分析内容，再翻译"
  - label: "Quick"
    description: "直接翻译，不做分析"
  - label: "Refined"
    description: "完整流程：分析 → 翻译 → 审校 → 润色"
```

### 问题 3：目标读者

```yaml
header: "读者"
question: "默认目标读者？"
options:
  - label: "大众读者（推荐）"
    description: "通俗表达，术语多作译者注"
  - label: "Technical"
    description: "开发者/工程师，常见技术词少注"
  - label: "Academic"
    description: "正式语体，术语精确"
  - label: "Business"
    description: "商务友好语气，技术概念会解释"
```

说明：用户也可输入自定义读者描述。

### 问题 4：翻译风格

```yaml
header: "风格"
question: "翻译风格？"
options:
  - label: "Storytelling（推荐）"
    description: "叙事感强、过渡顺畅"
  - label: "Formal"
    description: "专业、结构清晰、中性语气"
  - label: "Technical"
    description: "精确、文档体、简练"
  - label: "Literal"
    description: "贴近原文结构"
  - label: "Academic"
    description: "学术严谨、正式语体"
  - label: "Business"
    description: "简洁、结果导向、偏行动口吻"
  - label: "Humorous"
    description: "保留幽默，机智活泼"
  - label: "Conversational"
    description: "口语化、亲切自然"
  - label: "Elegant"
    description: "文学感、考究、用词讲究"
```

说明：用户也可输入自定义风格描述。

### 问题 5：保存位置

```yaml
header: "保存"
question: "偏好保存到哪里？"
options:
  - label: "用户（推荐）"
    description: "$HOME/.baoyu-skills/（所有项目）"
  - label: "项目"
    description: ".baoyu-skills/（仅本项目）"
```

## 保存位置

| 选项 | 路径 | 范围 |
|--------|------|-------|
| 用户 | `$HOME/.baoyu-skills/baoyu-translate/EXTEND.md` | 所有项目 |
| 项目 | `.baoyu-skills/baoyu-translate/EXTEND.md` | 当前项目 |

## 设置完成后

1. 必要时创建目录
2. 按所选值写入 EXTEND.md
3. 确认：「偏好已保存至 [path]」
4. 提示：「可随时在 EXTEND.md 中添加自定义术语，格式见文件中 `glossary` 一节。」
5. 使用已保存偏好继续翻译

## EXTEND.md 模板

```yaml
target_language: [zh-CN/zh-TW/en/ja/...]
default_mode: [quick/normal/refined]
audience: [general/technical/academic/business/custom]
style: [storytelling/formal/technical/literal/academic/business/humorous/conversational/elegant]

# Custom glossary (optional) — add your own term translations here
# glossary:
#   - from: "Term"
#     to: "翻译"
#   - from: "Another Term"
#     to: "另一个翻译"
#     note: "Usage context"
```

## 日后修改偏好

用户可直接编辑 EXTEND.md，或删除该文件以重新触发设置流程。
