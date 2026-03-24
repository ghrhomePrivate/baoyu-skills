# 工作流机制

源文件物化、输出目录创建与冲突处理的说明。

## 物化源内容

| 输入类型 | 操作 |
|------------|--------|
| 文件 | 直接使用（无需复制） |
| 内联文本 | 保存到 `translate/{slug}.md` |
| URL | 抓取内容，保存到 `translate/{slug}.md` |

`{slug}`：根据内容主题生成的 2–4 个词的 kebab-case 短标识。

## 创建输出目录

在源文件旁创建子目录：`{source-dir}/{source-basename}-{target-lang}/`

示例：
- `posts/article.md` → `posts/article-zh/`
- `translate/ai-future.md` → `translate/ai-future-zh/`

## 冲突处理

若输出目录已存在，先将现有目录重命名为 `{name}.backup-YYYYMMDD-HHMMSS/`，再创建新目录。切勿覆盖已有结果。
