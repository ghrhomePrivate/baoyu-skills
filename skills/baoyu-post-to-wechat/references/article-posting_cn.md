# 文章发表（Article Posting）

将 Markdown 文章发布到微信公众号，支持完整排版。

## 用法

```bash
# 发布 Markdown 文章
${BUN_X} ./scripts/wechat-article.ts --markdown article.md

# 指定主题
${BUN_X} ./scripts/wechat-article.ts --markdown article.md --theme grace

# 普通外链不转为文末引用，保持行内
${BUN_X} ./scripts/wechat-article.ts --markdown article.md --no-cite

# 显式指定选项
${BUN_X} ./scripts/wechat-article.ts --markdown article.md --author "作者名" --summary "摘要"
```

## 参数

| 参数 | 说明 |
|-----------|-------------|
| `--markdown <path>` | 要转换并发布的 Markdown 文件 |
| `--theme <name>` | 主题：default、grace、simple、modern |
| `--no-cite` | 普通外链保持行内，不转为文末引用 |
| `--title <text>` | 覆盖标题（默认从 Markdown 提取） |
| `--author <name>` | 作者名 |
| `--summary <text>` | 文章摘要 |
| `--html <path>` | 已渲染的 HTML 文件（与 markdown 二选一） |
| `--profile <dir>` | Chrome profile 目录 |

## Markdown 格式

```markdown
---
title: Article Title
author: Author Name
---

# Title (becomes article title)

Regular paragraph with **bold** and *italic*.

## Section Header

![Image description](./image.png)

- List item 1
- List item 2

> Blockquote text

[Link text](https://example.com)
```

Markdown 模式下，普通外链默认会转为文末引用，以便微信端展示。使用 `--no-cite` 可关闭该行为。

## 图片处理

1. **解析**：Markdown 中的图片替换为 `WECHATIMGPH_N`
2. **渲染**：生成带占位符的 HTML
3. **粘贴**：将 HTML 粘贴进微信编辑器
4. **替换**：对每个占位符：
   - 查找并选中占位文本
   - 滚动到可视区域
   - 按 Backspace 删除占位符
   - 从剪贴板粘贴图片

## 脚本

| 脚本 | 用途 |
|--------|---------|
| `wechat-article.ts` | 主文章发布脚本 |
| `md-to-wechat.ts` | Markdown 转带占位符的 HTML |
| `md/render.ts` | 带主题的 Markdown 渲染 |

## 示例会话

```
User: /post-to-wechat --markdown ./article.md

Claude:
1. 解析 Markdown，发现 5 张图片
2. 生成带占位符的 HTML
3. 打开 Chrome，进入微信编辑器
4. 粘贴 HTML 内容
5. 对每张图片：
   - 选中 WECHATIMGPH_1
   - 滚动到可视区域
   - 按 Backspace 删除占位
   - 粘贴图片
6. 汇报：「文章已排版完成，共 5 张图。」
```
