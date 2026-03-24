# 使用方法

## 命令语法

```bash
# 根据内容自动选择类型和风格
/baoyu-article-illustrator path/to/article.md

# 指定类型
/baoyu-article-illustrator path/to/article.md --type infographic

# 指定风格
/baoyu-article-illustrator path/to/article.md --style blueprint

# 同时指定类型和风格
/baoyu-article-illustrator path/to/article.md --type flowchart --style notion

# 指定密度
/baoyu-article-illustrator path/to/article.md --density rich

# 直接输入内容（粘贴模式）
/baoyu-article-illustrator
[粘贴内容]
```

## 选项

| 选项 | 描述 |
|------|------|
| `--type <name>` | 插图类型（参见 SKILL.md 中的类型画廊） |
| `--style <name>` | 视觉风格（参见 references/styles.md） |
| `--preset <name>` | 类型+风格组合的快捷方式（参见 [references/style-presets.md](references/style-presets.md)） |
| `--density <level>` | 图片数量：minimal / balanced / rich |

## 输入模式

| 模式 | 触发方式 | 输出目录 |
|------|----------|----------|
| 文件路径 | `path/to/article.md` | 使用 `default_output_dir` 偏好设置，未设置则询问 |
| 粘贴内容 | 无路径参数 | `illustrations/{topic-slug}/` |

## 输出目录选项

| 值 | 路径 |
|----|------|
| `same-dir` | `{article-dir}/` |
| `illustrations-subdir` | `{article-dir}/illustrations/` |
| `independent` | `illustrations/{topic-slug}/` |

在 EXTEND.md 中配置：`default_output_dir: illustrations-subdir`

## 示例

**带数据的技术文章**：
```bash
/baoyu-article-illustrator api-design.md --type infographic --style blueprint
```

**使用预设实现相同效果**：
```bash
/baoyu-article-illustrator api-design.md --preset tech-explainer
```

**个人故事**：
```bash
/baoyu-article-illustrator journey.md --preset storytelling
```

**带步骤的教程**：
```bash
/baoyu-article-illustrator how-to-deploy.md --preset tutorial --density rich
```

**观点文章配海报风格**：
```bash
/baoyu-article-illustrator opinion.md --preset opinion-piece
```

**预设加覆盖**：
```bash
/baoyu-article-illustrator article.md --preset tech-explainer --style notion
```
