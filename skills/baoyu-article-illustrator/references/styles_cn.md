# 风格参考

## 核心风格

用于快速选择的简化风格层级：

| 核心风格 | 对应风格 | 最适用于 |
|----------|----------|----------|
| `vector` | vector-illustration | 知识类文章、教程、技术内容 |
| `minimal-flat` | notion | 通用、知识分享、SaaS |
| `sci-fi` | blueprint | AI、前沿科技、系统设计 |
| `hand-drawn` | sketch/warm | 轻松、反思、休闲内容 |
| `editorial` | editorial | 流程、数据、新闻 |
| `scene` | warm/watercolor | 叙事、情感、生活方式 |
| `poster` | screen-print | 观点、评论、文化、电影感 |

大多数情况下使用核心风格。详细控制请参见下方完整风格画廊。

---

## 风格画廊

| 风格 | 描述 | 最适用于 |
|------|------|----------|
| `vector-illustration` | 简洁的扁平矢量艺术，粗犷形状 | 知识类文章、教程、技术内容 |
| `notion` | 极简手绘线条画 | 知识分享、SaaS、生产力 |
| `elegant` | 精致、优雅 | 商务、思想领导力 |
| `warm` | 友好、亲切 | 个人成长、生活方式、教育 |
| `minimal` | 超简洁、禅意 | 哲学、极简主义、核心概念 |
| `blueprint` | 技术蓝图 | 架构、系统设计、工程 |
| `watercolor` | 柔和的水彩艺术，自然温暖 | 生活方式、旅行、创意 |
| `editorial` | 杂志风信息图 | 技术解析、新闻 |
| `scientific` | 学术精密图表 | 生物、化学、技术研究 |
| `chalkboard` | 课堂黑板粉笔画风格 | 教育、教学、讲解 |
| `fantasy-animation` | 吉卜力/迪士尼风格手绘 | 童话、魔幻、情感 |
| `flat` | 现代粗犷几何形状 | 现代数字、当代 |
| `flat-doodle` | 可爱扁平风，粗轮廓线 | 可爱、友好、亲切 |
| `intuition-machine` | 技术简报风，做旧纸张 | 技术简报、学术 |
| `nature` | 有机自然的大地色插画 | 环保、健康 |
| `pixel-art` | 复古 8-bit 游戏美学 | 游戏、复古科技 |
| `playful` | 趣味粉彩涂鸦 | 有趣、休闲、教育 |
| `retro` | 80/90 年代霓虹几何 | 80/90 年代怀旧、大胆 |
| `sketch` | 原始铅笔笔记本风格 | 头脑风暴、创意探索 |
| `screen-print` | 粗犷海报艺术、半色调纹理、有限色彩 | 观点、评论、文化、电影感 |
| `sketch-notes` | 柔和手绘暖色笔记 | 教育、暖色笔记 |
| `vintage` | 做旧羊皮纸历史感 | 历史、传承 |

完整规格：`references/styles/<style>.md`

## 类型 × 风格兼容性矩阵

| | vector-illustration | notion | warm | minimal | blueprint | watercolor | elegant | editorial | scientific | screen-print |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| infographic | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓✓ | ✓ |
| scene | ✓ | ✓ | ✓✓ | ✓ | ✗ | ✓✓ | ✓ | ✓ | ✗ | ✓✓ |
| flowchart | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✗ | ✓ | ✓✓ | ✓ | ✗ |
| comparison | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ |
| framework | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✗ | ✓✓ | ✓ | ✓✓ | ✓ |
| timeline | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓✓ | ✓ | ✓ |

✓✓ = 强烈推荐 | ✓ = 兼容 | ✗ = 不推荐

## 按类型自动选择

| 类型 | 首选风格 | 备选风格 |
|------|----------|----------|
| infographic | vector-illustration | notion、blueprint、editorial |
| scene | warm | watercolor、elegant |
| flowchart | vector-illustration | notion、blueprint |
| comparison | vector-illustration | notion、elegant |
| framework | blueprint | vector-illustration、notion |
| timeline | elegant | warm、editorial |

## 按内容信号自动选择

| 内容信号 | 推荐类型 | 推荐风格 |
|----------|----------|----------|
| API、指标、数据、对比、数字 | infographic | blueprint、vector-illustration |
| 知识、概念、教程、学习、指南 | infographic | vector-illustration、notion |
| 技术、AI、编程、开发、代码 | infographic | vector-illustration、blueprint |
| 操作指南、步骤、工作流、流程、教程 | flowchart | vector-illustration、notion |
| 框架、模型、架构、原则 | framework | blueprint、vector-illustration |
| vs、优缺点、前后对比、替代方案 | comparison | vector-illustration、notion |
| 故事、情感、历程、体验、个人 | scene | warm、watercolor |
| 历史、时间线、进展、演进 | timeline | elegant、warm |
| 生产力、SaaS、工具、应用、软件 | infographic | notion、vector-illustration |
| 商务、专业、战略、企业 | framework | elegant |
| 观点、评论、文化、哲学、电影感、戏剧性、海报 | scene | screen-print |
| 生物、化学、医学、科学 | infographic | scientific |
| 解析、新闻、杂志、调查 | infographic | editorial |

## 风格特征（按类型）

### infographic + vector-illustration
- 简洁扁平矢量形状、粗犷几何图形
- 鲜艳但和谐的配色方案
- 清晰的视觉层次，搭配图标和标签
- 现代、专业、高度可读
- 非常适合知识类文章和教程

### flowchart + vector-illustration
- 粗箭头和连接线
- 带图标的独立步骤容器
- 清晰的进展流向
- 高对比度，便于阅读

### comparison + vector-illustration
- 分栏布局，清晰的视觉分隔
- 每侧使用粗图标
- 颜色编码区分
- 一目了然的对比效果

### framework + vector-illustration
- 几何节点表示
- 清晰的层级结构
- 粗连接线
- 现代系统图美学

### infographic + blueprint
- 技术精度，蓝图线条
- 基于网格的布局，清晰的区域
- 等宽标签，以数据为中心
- 蓝白配色方案

### infographic + notion
- 手绘质感，亲切感
- 柔和图标、圆角元素
- 中性配色、干净背景
- 非常适合 SaaS/生产力

### scene + warm
- 黄金时刻光线、温馨氛围
- 柔和渐变、自然纹理
- 邀请式的个人化感觉
- 非常适合讲故事

### scene + watercolor
- 艺术性、画作效果
- 柔和边缘、色彩晕染
- 梦幻、创意的情绪
- 最适合生活方式/旅行

### flowchart + notion
- 清晰的步骤指示
- 简单的箭头连接
- 极少装饰
- 聚焦流程清晰度

### flowchart + blueprint
- 技术精度
- 详细的连接点
- 工程美学
- 适合复杂系统

### comparison + elegant
- 精致的分隔符
- 均衡的排版
- 专业外观
- 商务对比

### framework + blueprint
- 精确的节点连接
- 层级清晰
- 系统架构感
- 技术框架

### timeline + elegant
- 精致的标记
- 优雅的排版
- 历史厚重感
- 专业演示

### timeline + warm
- 友好的进展
- 有机流动
- 个人历程感
- 成长叙事

### scene + screen-print
- 粗犷剪影、象征性构图
- 2-5 种平面颜色搭配半色调纹理
- 图底反转（负空间讲述次要故事）
- 复古海报美学，概念性而非字面性
- 非常适合观点文章和文化评论

### comparison + screen-print
- 分栏双色构图（每侧一种颜色）
- 粗犷几何分隔符
- 符号化图标优于详细渲染
- 高对比度，即时视觉冲击

### framework + screen-print
- 模版切割边缘的几何节点表示
- 有限的颜色编码（每个概念层级一种颜色）
- 简洁的剪影式图标
- 海报风格层次搭配粗体排版
