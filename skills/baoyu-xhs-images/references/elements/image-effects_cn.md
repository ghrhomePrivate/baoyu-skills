# 图像处理层

应用于小红书信息图中图像元素的视觉效果。

## AI Cutout（抠图）

产品/人物抠图的主体提取风格。

| 名称 | 说明 | 用途 |
|------|-------------|----------|
| clean | 边缘锐利、边界精确 | 产品图、数码类 |
| soft | 过渡柔和、羽化边缘 | 人像抠图、有机主体 |
| stylized | 手绘感边缘 | 艺术化构图 |

## Stroke Effects（描边）

抠图元素的描边处理。

| 名称 | 说明 | 用途 |
|------|-------------|----------|
| white-solid | 白色实线描边 | 经典贴纸感、高对比 |
| colored-solid | 彩色实线描边 | 活泼、品牌色 |
| dashed | 虚线/点线描边 | 手作感、轻松 |
| double | 双层描边 | 强调、高级感 |
| glow | 柔和外发光 | 梦幻、柔和气质 |
| shadow | 投影 | 层次、悬浮感 |

**描边粗细建议**：
- Thin：2-4px — 细腻、优雅
- Medium：5-8px — 常规可见度
- Thick：10-15px — 强强调

## Filters（滤镜）

小红书常见的调色与氛围预设。

| 名称 | Chinese | 说明 | 氛围 |
|------|---------|-------------|------|
| clear-glow | 清透感 | 通透、透亮、有光泽 | 清新、年轻 |
| film-grain | 胶片感 | 复古胶片、颗粒质感 | 怀旧、文艺 |
| cream-skin | 奶油肌 | 平滑奶油肌色调 | 柔和、显气色 |
| japanese-magazine | 日杂感 | 生活方式杂志感 | 精致、向往感 |
| high-saturation | 高饱和 | 鲜艳、冲击力强 | 有活力、吸睛 |
| muted-tones | 莫兰迪 | Morandi 式低饱和配色 | 高级、沉静 |
| warm-tone | 暖色调 | 黄金时段暖色 | 温馨、亲和 |
| cool-tone | 冷色调 | 偏蓝冷调 | 现代、干净 |

## 纹理叠加

附加纹理效果。

| 名称 | 说明 | 用途 |
|------|-------------|----------|
| paper | 纸张或织物纹理 | 手作感 |
| noise | 细腻颗粒噪点 | 模拟胶片感 |
| halftone | 网点图案 | 复古印刷 |
| scratch | 轻微划痕 | 复古磨损 |

## Blending Modes（混合模式）

分层合成时使用。

| 模式 | 效果 | 用途 |
|------|--------|----------|
| multiply | 压暗、叠色 | 阴影效果 |
| screen | 提亮、发光感 | 光效 |
| overlay | 增强对比 | 鲜艳合成 |
| soft-light | 柔和混合 | 自然叠层 |

## 效果组合

不同风格的常见效果组合：

### Cute 风格
- Filter：clear-glow 或 cream-skin
- Stroke：white-solid（medium）
- Texture：无

### Notion 风格
- Filter：无 或 muted-tones
- Stroke：white-solid（thin）或无
- Texture：paper（轻微）

### Retro 风格
- Filter：film-grain
- Stroke：double 或 dashed
- Texture：halftone、scratch

### Bold 风格
- Filter：high-saturation
- Stroke：colored-solid（thick）
- Texture：无
