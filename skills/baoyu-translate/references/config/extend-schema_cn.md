# baoyu-translate 的 EXTEND.md 字段说明

## 格式

EXTEND.md 使用 YAML：

```yaml
# 默认目标语言（ISO 代码或常用名称）
target_language: zh-CN

# 默认翻译模式
default_mode: normal  # quick | normal | refined

# 目标读者（影响注释深度与语体）
audience: general  # general | technical | academic | business | 或自定义字符串

# 翻译风格偏好
style: storytelling  # storytelling | formal | technical | literal | academic | business | humorous | conversational | elegant | 或自定义字符串

# 触发分块翻译的字数阈值
chunk_threshold: 4000

# 每块最大词数
chunk_max_words: 5000

# 自定义术语表（与内置术语表合并）
# CLI --glossary 会覆盖以下配置
# 支持行内条目和/或文件路径
glossary:
  - from: "Reinforcement Learning"
    to: "强化学习"
  - from: "Transformer"
    to: "Transformer"
    note: "Keep English"

# 从外部文件加载术语表
# 支持绝对路径或相对于 EXTEND.md 的路径
# 文件格式：含 | from | to | note | 列的 Markdown 表，
# 或 YAML 列表 {from, to, note}
glossary_files:
  - ./my-glossary.md
  - /path/to/shared-glossary.yaml

# 按语言对的术语表
glossaries:
  en-zh:
    - from: "AI Agent"
      to: "AI 智能体"
  ja-zh:
    - from: "人工知能"
      to: "人工智能"
```

## 字段

| 字段 | 类型 | 默认值 | 说明 |
|-------|------|---------|-------------|
| `target_language` | string | `zh-CN` | 默认目标语言代码 |
| `default_mode` | string | `normal` | 默认模式（`quick` / `normal` / `refined`） |
| `audience` | string | `general` | 目标读者（`general` / `technical` / `academic` / `business` / 自定义） |
| `style` | string | `storytelling` | 风格（`storytelling` / `formal` / `technical` / `literal` / `academic` / `business` / `humorous` / `conversational` / `elegant` / 自定义） |
| `chunk_threshold` | number | `4000` | 触发分块翻译的字数阈值 |
| `chunk_max_words` | number | `5000` | 每块最大词数 |
| `glossary` | array | `[]` | 通用术语（行内） |
| `glossary_files` | array | `[]` | 外部术语表路径（绝对或相对 EXTEND.md） |
| `glossaries` | object | `{}` | 按语言对的术语条目 |

## 术语条目

| 字段 | 必填 | 说明 |
|-------|----------|-------------|
| `from` | 是 | 源语言术语 |
| `to` | 是 | 目标语译文 |
| `note` | 否 | 使用说明（如 "Keep English"、"仅技术语境"） |

## 术语表文件格式

外部术语表（`glossary_files`）支持两种格式：

**Markdown 表**（`.md`）：
```markdown
| from | to | note |
|------|----|------|
| Reinforcement Learning | 强化学习 | |
| Transformer | Transformer | Keep English |
```

**YAML 列表**（`.yaml` / `.yml`）：
```yaml
- from: "Reinforcement Learning"
  to: "强化学习"
- from: "Transformer"
  to: "Transformer"
  note: "Keep English"
```

路径可为绝对路径，或相对于 EXTEND.md 所在目录。

## 优先级

1. CLI `--glossary` 文件中的条目
2. EXTEND.md 中 `glossaries[pair]` 的条目
3. EXTEND.md 中 `glossary`（行内）
4. EXTEND.md 中 `glossary_files`（按列出顺序，后文件覆盖先文件）
5. 内置术语表（如 `references/glossary-en-zh.md`）

同一源术语，靠后的条目覆盖靠前的。
