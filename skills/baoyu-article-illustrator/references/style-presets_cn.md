# 风格预设

`--preset X` 会展开为一个类型+风格的组合。用户可以覆盖其中任一维度。

## 按类别分类

### 技术与工程

| --preset | 类型 | 风格 | 最适用于 |
|----------|------|------|----------|
| `tech-explainer` | `infographic` | `blueprint` | API 文档、系统指标、技术深度解析 |
| `system-design` | `framework` | `blueprint` | 架构图、系统设计 |
| `architecture` | `framework` | `vector-illustration` | 组件关系、模块结构 |
| `science-paper` | `infographic` | `scientific` | 研究成果、实验结果、学术 |

### 知识与教育

| --preset | 类型 | 风格 | 最适用于 |
|----------|------|------|----------|
| `knowledge-base` | `infographic` | `vector-illustration` | 概念讲解、教程、操作指南 |
| `saas-guide` | `infographic` | `notion` | 产品指南、SaaS 文档、工具演练 |
| `tutorial` | `flowchart` | `vector-illustration` | 分步教程、安装指南 |
| `process-flow` | `flowchart` | `notion` | 工作流文档、入职流程 |

### 数据与分析

| --preset | 类型 | 风格 | 最适用于 |
|----------|------|------|----------|
| `data-report` | `infographic` | `editorial` | 数据新闻、指标报告、仪表盘 |
| `versus` | `comparison` | `vector-illustration` | 技术对比、框架评测 |
| `business-compare` | `comparison` | `elegant` | 产品评估、战略选项 |

### 叙事与创意

| --preset | 类型 | 风格 | 最适用于 |
|----------|------|------|----------|
| `storytelling` | `scene` | `warm` | 个人随笔、感悟、成长故事 |
| `lifestyle` | `scene` | `watercolor` | 旅行、健康、生活方式、创意 |
| `history` | `timeline` | `elegant` | 历史概述、里程碑 |
| `evolution` | `timeline` | `warm` | 进展叙事、成长历程 |

### 评论与观点

| --preset | 类型 | 风格 | 最适用于 |
|----------|------|------|----------|
| `opinion-piece` | `scene` | `screen-print` | 评论文章、时评、批判性文章 |
| `editorial-poster` | `comparison` | `screen-print` | 辩论、对立观点 |
| `cinematic` | `scene` | `screen-print` | 戏剧性叙事、文化评论 |

## 内容类型 → 预设推荐

在步骤 3 中根据步骤 2 的内容分析使用此表推荐预设：

| 内容类型（步骤 2） | 首选预设 | 备选 |
|--------------------|----------|------|
| 技术 | `tech-explainer` | `system-design`、`architecture` |
| 教程 | `tutorial` | `process-flow`、`knowledge-base` |
| 方法论 / 框架 | `system-design` | `architecture`、`process-flow` |
| 数据 / 指标 | `data-report` | `versus`、`tech-explainer` |
| 对比 / 评测 | `versus` | `business-compare`、`editorial-poster` |
| 叙事 / 个人 | `storytelling` | `lifestyle`、`evolution` |
| 观点 / 评论 | `opinion-piece` | `cinematic`、`editorial-poster` |
| 历史 / 时间线 | `history` | `evolution` |
| 学术 / 研究 | `science-paper` | `tech-explainer`、`data-report` |
| SaaS / 产品 | `saas-guide` | `knowledge-base`、`process-flow` |

## 覆盖示例

- `--preset tech-explainer --style notion` = infographic 类型 + notion 风格
- `--preset storytelling --type timeline` = timeline 类型 + warm 风格

显式的 `--type`/`--style` 标志始终覆盖预设值。
