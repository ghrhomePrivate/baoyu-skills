# 贴图发表（Image-Text Posting，原「图文」）

向微信公众号发布含多张图片的贴图消息。

> **说明**：截至 2026 年，公众号后台已将「图文」更名为「贴图」。

## 用法

```bash
# 使用 Markdown 与图片目录（标题/正文自动提取）
${BUN_X} ./scripts/wechat-browser.ts --markdown source.md --images ./images/

# 显式指定标题与正文
${BUN_X} ./scripts/wechat-browser.ts --title "标题" --content "内容" --image img1.png --image img2.png

# 存为草稿
${BUN_X} ./scripts/wechat-browser.ts --markdown source.md --images ./images/ --submit
```

## 参数

| 参数 | 说明 |
|-----------|-------------|
| `--markdown <path>` | 用于提取标题/正文的 Markdown 文件 |
| `--images <dir>` | 图片所在目录（按文件名排序） |
| `--title <text>` | 标题（最多 20 字，过长自动压缩） |
| `--content <text>` | 正文（最多 1000 字，过长自动压缩） |
| `--image <path>` | 单张图片（可重复） |
| `--submit` | 存为草稿（默认仅预览） |
| `--profile <dir>` | Chrome profile 目录 |

## 从 Markdown 自动提取标题/正文

使用 `--markdown` 时，脚本会：

1. **解析 frontmatter** 中的 title、author：
   ```yaml
   ---
   title: 文章标题
   author: 作者名
   ---
   ```

2. **若无 frontmatter 标题则回退到 H1**：
   ```markdown
   # 这将成为标题
   ```

3. **标题过长时压缩至 20 字**：
   - 原文：「如何在一天内彻底重塑你的人生」
   - 压缩后：「一天彻底重塑你的人生」

4. **取开头段落作为正文**（最多 1000 字）

## 图片目录模式

使用 `--images <dir>` 时：

- 上传目录内所有 PNG/JPG
- 按文件名字母序排序
- 命名建议：`01-cover.png`、`02-content.png` 等

## 限制

| 字段 | 最大长度 | 说明 |
|-------|------------|-------|
| Title | 20 chars | 过长自动压缩 |
| Content | 1000 chars | 过长自动压缩 |
| Images | 9 max | 微信上限 |

## 示例会话

```
User: /post-to-wechat --markdown ./article.md --images ./xhs-images/

Claude:
1. 解析 Markdown 元信息：
   - 标题：「如何在一天内彻底重塑你的人生」→「一天内重塑你的人生」
   - 作者：来自 frontmatter 或默认值
2. 从开头段落提取正文
3. 在 xhs-images/ 中发现 7 张图
4. 打开 Chrome，进入微信「图文」编辑器
5. 上传全部图片
6. 填写标题与正文
7. 汇报：「贴图已填写，共 7 张图。」
```

## 脚本

| 脚本 | 用途 |
|--------|---------|
| `wechat-browser.ts` | 贴图发布主脚本 |
| `cdp.ts` | Chrome DevTools Protocol 工具 |
| `copy-to-clipboard.ts` | 剪贴板操作 |
