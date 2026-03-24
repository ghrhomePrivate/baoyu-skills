# 布局库

单张幻灯片的可选布局提示。在大纲的 `// LAYOUT` 部分中指定。

## 幻灯片特定布局

| 布局 | 描述 | 适用场景 |
|--------|-------------|----------|
| `title-hero` | 居中大标题 + 副标题 | 封面幻灯片、章节分隔 |
| `quote-callout` | 突出引言带出处 | 推荐语、关键洞察 |
| `key-stat` | 单个大数字作为焦点 | 冲击力统计数据、指标 |
| `split-screen` | 一半图像、一半文字 | 功能亮点、对比 |
| `icon-grid` | 带标签的图标网格 | 功能、能力、优势 |
| `two-columns` | 均衡的两栏内容 | 配对信息、双要点 |
| `three-columns` | 三栏内容 | 三方对比、分类 |
| `image-caption` | 全出血图像 + 文字叠加 | 视觉叙事、情感化 |
| `agenda` | 带高亮的编号列表 | 议程概览、路线图 |
| `bullet-list` | 结构化要点列表 | 简单内容、列表 |

## 信息图衍生布局

| 布局 | 描述 | 适用场景 |
|--------|-------------|----------|
| `linear-progression` | 从左到右的顺序流程 | 时间线、分步骤 |
| `binary-comparison` | A vs B 并排对比 | 前后对比、利弊分析 |
| `comparison-matrix` | 多因素网格 | 功能对比 |
| `hierarchical-layers` | 金字塔或堆叠层级 | 优先级、重要性 |
| `hub-spoke` | 中心节点辐射项目 | 概念图、生态系统 |
| `bento-grid` | 不同大小的磁贴 | 概览、总结 |
| `funnel` | 逐步收窄的阶段 | 转化、筛选 |
| `dashboard` | 带图表/数字的指标 | KPI、数据展示 |
| `venn-diagram` | 重叠圆圈 | 关系、交集 |
| `circular-flow` | 连续循环 | 循环流程 |
| `winding-roadmap` | 带里程碑的弯曲路径 | 旅程、时间线 |
| `tree-branching` | 父子层级结构 | 组织图、分类法 |
| `iceberg` | 可见层与隐藏层 | 表面与深层 |
| `bridge` | 带连接的间隙 | 问题-解决方案 |

**用法**：在幻灯片的 `// LAYOUT` 部分添加 `Layout: <name>`。

## 布局选择技巧

**将布局与内容匹配**：
| 内容类型 | 推荐布局 |
|--------------|-------------------|
| 单一叙事 | `bullet-list`、`image-caption` |
| 两个概念 | `split-screen`、`binary-comparison` |
| 三个项目 | `three-columns`、`icon-grid` |
| 流程/步骤 | `linear-progression`、`winding-roadmap` |
| 数据/指标 | `dashboard`、`key-stat` |
| 关系 | `hub-spoke`、`venn-diagram` |
| 层级 | `hierarchical-layers`、`tree-branching` |

**布局流程模式**：
| 位置 | 推荐布局 |
|----------|-------------------|
| 开场 | `title-hero`、`agenda` |
| 中间 | 内容特定布局 |
| 结尾 | `quote-callout`、`key-stat` |

**常见错误避免**：
- 对 2 个项目使用三栏布局（会留空栏）
- 在文字下方堆叠图表/表格（改用并排布局）
- 没有实际图片的图像布局
- 对非引言使用引言布局（仅用于带出处的真实引言）
