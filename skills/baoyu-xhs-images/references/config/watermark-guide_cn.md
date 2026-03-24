---
name: watermark-guide
description: baoyu-xhs-images 水印配置指南
---

# 水印指南

## 位置示意图

```
┌─────────────────────────────┐
│                  [top-right]│
│                             │
│                             │
│         IMAGE CONTENT       │
│                             │
│                             │
│[bottom-left][bottom-center][bottom-right]│
└─────────────────────────────┘
```

## 位置建议

| 位置 | 适用 | 避免使用场景 |
|------|------|--------------|
| `bottom-right` | 默认、最常用 | 右下角是关键信息时 |
| `bottom-left` | 右侧偏重的版式 | 左下角是关键信息时 |
| `bottom-center` | 居中设计 | 底部文字很多时 |
| `top-right` | 底部内容很重时 | 标题/头图在右上角时 |

## 内容格式

| 格式 | 示例 | 说明 |
|------|------|------|
| Handle | `@username` | 小红书最常见 |
| Text | `MyBrand` | 简单品牌字 |
| Chinese | `小红书:用户名` | 平台向 |
| URL | `myblog.com` | 跨平台 |

## 最佳实践

1. **一致性**：系列内各图使用相同水印
2. **可读性**：在浅色/深色区域都应能看清
3. **尺寸**：保持低调，不抢主内容

## 与 Prompt 结合

启用水印时，在出图 prompt 中加入：

```
Include a subtle watermark "[content]" positioned at [position].
The watermark should be legible but not distracting from the main content.
```

## 常见问题

| 问题 | 处理 |
|------|------|
| 水印看不见 | 调整位置或检查对比度 |
| 水印太抢眼 | 换位置或缩小 |
| 水印压住内容 | 换位置 |
| 各图不一致 | 使用 session ID 保持一致性 |
