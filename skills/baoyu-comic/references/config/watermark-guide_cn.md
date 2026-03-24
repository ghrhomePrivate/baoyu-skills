---
name: watermark-guide
description: baoyu-comic 的水印配置指南
---

# 水印指南

## 位置示意图

```
┌─────────────────────────────┐
│                  [top-right]│ ← 避免使用（与页码冲突）
│                             │
│                             │
│       漫画页面内容           │
│                             │
│                             │
│[bottom-left][bottom-center][bottom-right]│
└─────────────────────────────┘
```

## 位置推荐

| 位置 | 适合场景 | 避免场景 |
|----------|----------|------------|
| `bottom-right` | 默认选择，适用于大多数分格布局 | 右下角有关键分格时 |
| `bottom-left` | 右侧偏重的布局 | 左下角有关键分格时 |
| `bottom-center` | Webtoon 竖版滚动，居中设计 | 底部文字密集区域 |
| `top-right` | **不推荐用于漫画** | 始终避免 - 与页码冲突 |

## 内容格式

| 格式 | 示例 | 风格 |
|--------|---------|-------|
| 用户名 | `@username` | 社交媒体风格 |
| 文字 | `Studio Name` | 专业品牌 |
| 中文 | `漫画工作室` | 中文市场 |
| 首字母 | `ABC` | 简约、干净 |

## 漫画水印最佳实践

1. **感知分格位置**：避免覆盖对话气泡或关键动作
2. **一致性**：漫画所有页面使用相同水印
3. **尺寸**：保持低调——不应干扰叙事
4. **风格匹配**：水印风格应与漫画视觉风格协调
5. **Webtoon 特殊处理**：竖版滚动格式使用 `bottom-center`

## 提示词集成

当水印启用时，添加到图像生成提示词中：

```
Include a subtle watermark "[content]" positioned at [position].
The watermark should be legible but not distracting from the comic panels
and storytelling. Ensure watermark does not overlap speech bubbles or key action.
```

## 常见问题

| 问题 | 解决方案 |
|-------|----------|
| 深色分格上水印不可见 | 调整对比度或添加微妙轮廓 |
| 水印与对话气泡重叠 | 更改位置或下移 |
| 各页水印不一致 | 使用 session ID 保持一致性 |
| 水印过于突出 | 更改位置或缩小尺寸 |
| 与页码冲突 | 永远不要使用右上角位置 |
