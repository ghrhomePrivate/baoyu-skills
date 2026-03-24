# X Articles — 详细指南

将 Markdown 文章发布到 X Articles 编辑器，支持富文本与图片。

## 前置条件

- X Premium 订阅（Articles 必需）
- 已安装 Google Chrome
- 已安装 `bun`

## 用法

```bash
# 发布 Markdown 文章（预览模式）
${BUN_X} {baseDir}/scripts/x-article.ts article.md

# 自定义封面图
${BUN_X} {baseDir}/scripts/x-article.ts article.md --cover ./cover.jpg

# 实际发布
${BUN_X} {baseDir}/scripts/x-article.ts article.md --submit
```

## Markdown 格式

```markdown
---
title: My Article Title
cover_image: /path/to/cover.jpg
---

# Title (becomes article title)

Regular paragraph text with **bold** and *italic*.

## Section Header

More content here.

![Image alt text](./image.png)

- List item 1
- List item 2

1. Numbered item
2. Another item

> Blockquote text

[Link text](https://example.com)

\`\`\`
Code blocks become blockquotes (X doesn't support code)
\`\`\`
```

## Frontmatter 字段

| 字段 | 说明 |
|-------|-------------|
| `title` | 文章标题（或使用首个 H1） |
| `cover_image` | 封面路径或 URL |
| `cover` | `cover_image` 的别名 |
| `image` | `cover_image` 的别名 |

## 图片处理

1. **封面**：正文首张图或 frontmatter 中的 `cover_image`
2. **远程图片**：自动下载到临时目录
3. **占位符**：正文图片使用 `XIMGPH_N` 格式
4. **插入**：定位占位符、选中并替换为真实图片

## Markdown 转 HTML 脚本

转换 Markdown 并查看结构：

```bash
# 输出含全部元数据的 JSON
${BUN_X} {baseDir}/scripts/md-to-html.ts article.md

# 仅输出 HTML
${BUN_X} {baseDir}/scripts/md-to-html.ts article.md --html-only

# 将 HTML 写入文件
${BUN_X} {baseDir}/scripts/md-to-html.ts article.md --save-html /tmp/article.html
```

JSON 输出示例：
```json
{
  "title": "Article Title",
  "coverImage": "/path/to/cover.jpg",
  "contentImages": [
    {
      "placeholder": "XIMGPH_1",
      "localPath": "/tmp/x-article-images/img.png",
      "blockIndex": 5
    }
  ],
  "html": "<p>Content...</p>",
  "totalBlocks": 20
}
```

## 支持的排版

| Markdown | HTML 输出 |
|----------|-------------|
| `# H1` | 仅作标题（不出现在正文） |
| `## H2` - `###### H6` | `<h2>` |
| `**bold**` | `<strong>` |
| `*italic*` | `<em>` |
| `[text](url)` | `<a href>` |
| `> quote` | `<blockquote>` |
| `` `code` `` | `<code>` |
| ```` ``` ```` | `<blockquote>`（X 限制） |
| `- item` | `<ul><li>` |
| `1. item` | `<ol><li>` |
| `![](img)` | 图片占位符 |

## 工作流

1. **解析 Markdown**：提取标题、封面、正文图片，生成 HTML
2. **启动 Chrome**：真实浏览器 + CDP，持久登录
3. **导航**：打开 `x.com/compose/articles`
4. **创建文章**：若在列表页则点击创建
5. **上传封面**：通过 file input 上传封面
6. **填写标题**：输入标题字段
7. **粘贴正文**：HTML 复制到剪贴板后粘贴进编辑器
8. **插入图片**：对每个占位符（逆序）：
   - 在编辑器中找到占位文本
   - 选中占位符
   - 将图片复制到剪贴板
   - 粘贴替换选中内容
9. **合成后检查**（自动）：
   - 扫描编辑器中残留的 `XIMGPH_` 占位符
   - 对比预期与实际图片数量
   - 发现问题则警告
10. **预览**：浏览器保持打开约 60 秒供预览
11. **发布**：仅在使用 `--submit` 时发布

## 示例会话

```
User: /post-to-x article ./blog/my-post.md --cover ./thumbnail.png

Claude:
1. 解析 Markdown：标题 "My Post"，正文 3 张图
2. 以 CDP 启动 Chrome
3. 打开 x.com/compose/articles
4. 点击创建按钮
5. 将 thumbnail.png 作为封面上传
6. 填写标题 "My Post"
7. 粘贴 HTML 正文
8. 在占位符位置插入 3 张图
9. 汇报：「长文已排版。请预览，使用 --submit 发布。」
```

## 故障排除

- **没有创建按钮**：确认 X Premium 有效
- **封面上传失败**：检查路径与格式（PNG、JPEG）
- **图片未插入**：确认粘贴内容中含占位符
- **内容未粘贴**：检查 HTML 剪贴板：`${BUN_X} {baseDir}/scripts/copy-to-clipboard.ts html --file /tmp/test.html`

## 实现原理

1. `md-to-html.ts` 将 Markdown 转为 HTML：
   - 提取 frontmatter（title、cover）
   - Markdown → HTML
   - 图片替换为唯一占位符
   - 远程图片下载到本地
   - 返回结构化 JSON

2. `x-article.ts` 通过 CDP 发布：
   - 启动真实 Chrome（降低检测）
   - 使用持久 profile（保留登录）
   - DOM 操作导航并填写编辑器
   - 通过系统剪贴板粘贴 HTML
   - 查找/选中/替换每个图片占位符
