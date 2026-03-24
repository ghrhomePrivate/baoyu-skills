---
name: first-time-setup
description: baoyu-post-to-wechat 偏好首次设置流程
---

# 首次设置

## 概述

未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：必须先完成本设置，再执行任何其他工作流步骤。禁止：
- 询问要发布的内容或文件
- 询问主题或发布方式
- 进入内容转换或发布流程

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
    Continue to Step 1
```

## 问题

**语言**：使用用户输入语言或已保存的语言偏好。

在**一次** AskUserQuestion 调用中包含**全部**问题：

### 问题 1：默认主题

```yaml
header: "主题"
question: "文章转换的默认主题？"
options:
  - label: "default（推荐）"
    description: "经典版式——居中标题与边框、彩色底白字 H2（默认蓝色）"
  - label: "grace"
    description: "雅致——文字阴影、圆角卡片、精致引用（默认紫色）"
  - label: "simple"
    description: "极简现代——不对称圆角、留白干净（默认绿色）"
  - label: "modern"
    description: "大圆角、胶囊标题、留白宽松（默认橙色）"
```

### 问题 2：默认配色

```yaml
header: "配色"
question: "默认配色预设？（不选则用主题内置默认）"
options:
  - label: "主题默认（推荐）"
    description: "使用主题内置默认色"
  - label: "blue"
    description: "#0F4C81 经典蓝"
  - label: "red"
    description: "#A93226 中国红"
  - label: "green"
    description: "#009874 翡翠绿"
```

说明：用户可选择「其他」并输入任意预设名（vermilion、yellow、purple、sky、rose、olive、black、gray、pink、orange）或十六进制色值。

### 问题 3：默认发布方式

```yaml
header: "方式"
question: "默认发布方式？"
options:
  - label: "api（推荐）"
    description: "较快，需 API 凭据（AppID + AppSecret）"
  - label: "browser"
    description: "较慢，需 Chrome 与登录会话"
```

### 问题 4：默认作者

```yaml
header: "作者"
question: "文章默认作者名？"
options:
  - label: "无默认"
    description: "留空，按篇指定"
```

说明：用户通常会选「其他」并输入作者名。

### 问题 5：开启留言

```yaml
header: "留言"
question: "文章默认开启读者留言？"
options:
  - label: "是（推荐）"
    description: "允许读者留言"
  - label: "否"
    description: "默认关闭留言"
```

### 问题 6：仅粉丝可留言

```yaml
header: "仅粉丝"
question: "是否限制为仅关注者可留言？"
options:
  - label: "否（推荐）"
    description: "所有读者可留言"
  - label: "是"
    description: "仅关注者可留言"
```

### 问题 7：保存位置

```yaml
header: "保存"
question: "偏好保存到哪里？"
options:
  - label: "项目（推荐）"
    description: ".baoyu-skills/（仅本项目）"
  - label: "用户"
    description: "~/.baoyu-skills/（所有项目）"
```

## 保存位置

| 选项 | 路径 | 范围 |
|--------|------|-------|
| Project | `.baoyu-skills/baoyu-post-to-wechat/EXTEND.md` | 当前项目 |
| User | `~/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md` | 所有项目 |

## 设置完成后

1. 必要时创建目录
2. 写入 EXTEND.md
3. 确认：「偏好已保存至 [path]」
4. 继续 Step 0（加载已保存的偏好）

## EXTEND.md 模板

### 单账号（默认）

```md
default_theme: [default/grace/simple/modern]
default_color: [preset name, hex, or empty for theme default]
default_publish_method: [api/browser]
default_author: [author name or empty]
need_open_comment: [1/0]
only_fans_can_comment: [1/0]
chrome_profile_path:
```

### 多账号

```md
default_theme: [default/grace/simple/modern]
default_color: [preset name, hex, or empty for theme default]

accounts:
  - name: [display name]
    alias: [short key, e.g. "baoyu"]
    default: true
    default_publish_method: [api/browser]
    default_author: [author name]
    need_open_comment: [1/0]
    only_fans_can_comment: [1/0]
    app_id: [WeChat App ID, optional]
    app_secret: [WeChat App Secret, optional]
  - name: [second account name]
    alias: [short key, e.g. "ai-tools"]
    default_publish_method: [api/browser]
    default_author: [author name]
    need_open_comment: [1/0]
    only_fans_can_comment: [1/0]
```

## 后续添加更多账号

首次设置后，用户可编辑 EXTEND.md 增加账号：

1. 添加 `accounts:` 列表块
2. 将按账号的设置（作者、发布方式、留言等）移到各账号条目下
3. 全局设置（主题、配色）保留在顶层
4. 每个账号需要唯一 `alias`（用于 CLI `--account` 与 Chrome profile 命名）
5. 在主账号上设置 `default: true`

## 日后修改偏好

用户可直接编辑 EXTEND.md，或删除该文件以重新触发设置流程。
