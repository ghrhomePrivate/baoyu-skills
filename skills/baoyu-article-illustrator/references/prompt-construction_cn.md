# 提示词构建

## 提示词文件格式

每个提示词文件使用 YAML frontmatter + 内容：

```yaml
---
illustration_id: 01
type: infographic
style: blueprint
references:                    # ⚠️ 仅当文件确实存在于 references/ 目录中时
  - ref_id: 01
    filename: 01-ref-diagram.png
    usage: direct              # direct | style | palette
---

[下方为类型专用模板内容...]
```

**⚠️ 关键 - 何时包含 `references` 字段**：

| 情况 | 操作 |
|------|------|
| 参考文件已保存到 `references/` | 在 frontmatter 中包含 ✓ |
| 风格通过口头描述提取（无文件） | 不要包含在 frontmatter 中，改为追加到提示词正文 |
| frontmatter 中有文件路径但文件不存在 | 错误 - 移除 references 字段 |

**参考素材使用类型**（仅当文件存在时）：

| 用途 | 描述 | 生成操作 |
|------|------|----------|
| `direct` | 主要视觉参考 | 传递给 `--ref` 参数 |
| `style` | 仅提取风格特征 | 在提示词文本中描述风格 |
| `palette` | 提取配色方案 | 在提示词中包含颜色 |

**如果没有参考文件但通过口头描述提取了风格/配色**，直接追加到提示词正文：
```
COLORS（来自参考）:
- Primary: #E8756D 珊瑚色
- Secondary: #7ECFC0 薄荷色
...

STYLE（来自参考）:
- 简洁线条、极少阴影
- 渐变背景
...
```

---

## 默认构图要求

**默认应用于所有提示词**：

| 要求 | 描述 |
|------|------|
| **简洁构图** | 简单布局，无视觉杂乱 |
| **留白** | 充足的边距，元素间有呼吸空间 |
| **无复杂背景** | 仅使用纯色或微妙渐变，避免繁杂纹理 |
| **居中或内容适配** | 主要视觉元素居中或按内容需要定位 |
| **匹配图形** | 使用与内容主题一致的图形元素 |
| **突出核心信息** | 通过留白引导注意力到关键信息 |

**添加到所有提示词中**：
> 简洁构图，充足留白。简单或无背景。主要元素居中或按内容需要定位。

---

## 人物渲染

描绘人物时：

| 准则 | 描述 |
|------|------|
| **风格** | 简化的卡通剪影或符号化表达 |
| **避免** | 写实人物形象、详细面部特征 |
| **多样性** | 显示多个人物时使用多样化的体型 |
| **情感** | 通过姿势和简单手势表达 |

**添加到所有包含人物的提示词中**：
> 人物形象：简化的风格化剪影或符号化表示，非写实。

---

## 插图中的文字

| 元素 | 准则 |
|------|------|
| **大小** | 大号、醒目、立即可读 |
| **风格** | 优先使用手写字体以增加温暖感 |
| **内容** | 仅保留简洁的关键词和核心概念 |
| **语言** | 与文章语言一致 |

**添加到包含文字的提示词中**：
> 文字应大号醒目，使用手写风格字体。保持最少量，聚焦关键词。

---

## 原则

好的提示词必须包含：

1. **布局结构优先**：描述构图、区域、流向
2. **具体数据/标签**：使用文章中的实际数字和术语
3. **视觉关系**：元素之间如何连接
4. **语义化颜色**：基于含义的颜色选择（红色=警告，绿色=高效）
5. **风格特征**：线条处理、纹理、情绪
6. **宽高比**：以比例和复杂度级别结尾

## 类型专用模板

### Infographic（信息图）

```
[标题] - 数据可视化

Layout: [grid/radial/hierarchical]

ZONES:
- Zone 1: [带具体数值的数据点]
- Zone 2: [带指标的对比]
- Zone 3: [总结/结论]

LABELS: [文章中的具体数字、百分比、术语]
COLORS: [语义化颜色映射]
STYLE: [风格特征]
ASPECT: 16:9
```

**Infographic + vector-illustration**：
```
扁平矢量插画信息图。所有元素使用清晰的黑色轮廓线。
COLORS: 奶油色背景 (#F5F0E6)、珊瑚红 (#E07A5F)、薄荷绿 (#81B29A)、芥末黄 (#F2CC8F)
ELEMENTS: 几何简化图标、无渐变、趣味装饰元素（圆点、星星）
```

### Scene（场景）

```
[标题] - 氛围场景

FOCAL POINT: [主体]
ATMOSPHERE: [光线、情绪、环境]
MOOD: [要传达的情感]
COLOR TEMPERATURE: [暖/冷/中性]
STYLE: [风格特征]
ASPECT: 16:9
```

### Flowchart（流程图）

```
[标题] - 流程图

Layout: [left-right/top-down/circular]

STEPS:
1. [步骤名称] - [简要描述]
2. [步骤名称] - [简要描述]
...

CONNECTIONS: [箭头类型、决策点]
STYLE: [风格特征]
ASPECT: 16:9
```

**Flowchart + vector-illustration**：
```
扁平矢量流程图，使用粗箭头和几何步骤容器。
COLORS: 奶油色背景 (#F5F0E6)，步骤使用珊瑚色/薄荷色/芥末色，黑色轮廓线
ELEMENTS: 圆角矩形、粗箭头、每步配简单图标
```

### Comparison（对比图）

```
[标题] - 对比视图

LEFT SIDE - [选项 A]:
- [要点 1]
- [要点 2]

RIGHT SIDE - [选项 B]:
- [要点 1]
- [要点 2]

DIVIDER: [视觉分隔符]
STYLE: [风格特征]
ASPECT: 16:9
```

**Comparison + vector-illustration**：
```
扁平矢量对比图，分栏布局。清晰的视觉分隔。
COLORS: 左侧珊瑚色 (#E07A5F)，右侧薄荷色 (#81B29A)，奶油色背景
ELEMENTS: 粗图标、黑色轮廓线、居中分隔线
```

### Framework（框架图）

```
[标题] - 概念框架

STRUCTURE: [hierarchical/network/matrix]

NODES:
- [概念 1] - [角色]
- [概念 2] - [角色]

RELATIONSHIPS: [节点之间的连接方式]
STYLE: [风格特征]
ASPECT: 16:9
```

**Framework + vector-illustration**：
```
扁平矢量框架图，使用几何节点和粗连接线。
COLORS: 奶油色背景 (#F5F0E6)，节点使用珊瑚色/薄荷色/芥末色/蓝色，黑色轮廓线
ELEMENTS: 圆角矩形或圆形节点、粗连接线
```

### Timeline（时间线）

```
[标题] - 时间视图

DIRECTION: [horizontal/vertical]

EVENTS:
- [日期/时期 1]: [里程碑]
- [日期/时期 2]: [里程碑]

MARKERS: [视觉标记]
STYLE: [风格特征]
ASPECT: 16:9
```

### Screen-Print 风格覆盖

当 `style: screen-print` 时，将标准风格指令替换为：

```
丝网印刷/丝印海报艺术。平面色块，无渐变。
COLORS: 最多 2-5 种颜色。[从风格调色板或双色配对中选择]
TEXTURE: 半色调网点图案、轻微的颜色层错位、纸张纹理
COMPOSITION: 粗犷剪影、几何构图、负空间作为叙事元素
FIGURES: 仅剪影，无详细面部，模版切割边缘
TYPOGRAPHY: 粗体窄体无衬线字体融入构图中（非叠加覆盖）
```

**Scene + screen-print**：
```
概念海报场景。单一象征性焦点，非字面插图。
COLORS: 双色配对（例如，焦橙 #E8751A + 深青 #0A6E6E）基于黑底 #121212
COMPOSITION: 居中剪影或几何框架，60%+ 负空间
TEXTURE: 半色调网点、纸张纹理、轻微印刷错位
```

**Comparison + screen-print**：
```
分栏海报构图。每侧由双色配对中的一种颜色主导。
LEFT: [颜色 A] 侧配 [选项 A] 的剪影/图标
RIGHT: [颜色 B] 侧配 [选项 B] 的剪影/图标
DIVIDER: 几何形状或负空间边界
TEXTURE: 两侧之间的半色调过渡
```

---

## 应避免的事项

- 模糊描述（"一张好看的图"）
- 字面隐喻插图
- 缺少具体标签/注释
- 通用装饰元素

## 水印集成

如果偏好设置中启用了水印，追加：

```
Include a subtle watermark "[content]" positioned at [position] with approximately [opacity*100]% visibility.
```
