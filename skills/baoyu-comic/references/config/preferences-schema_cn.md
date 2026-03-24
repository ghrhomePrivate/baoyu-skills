---
name: preferences-schema
description: baoyu-comic 用户偏好的 EXTEND.md YAML schema
---

# 偏好设置 Schema

## 完整 Schema

```yaml
---
version: 2

watermark:
  enabled: false
  content: ""
  position: bottom-right  # bottom-right|bottom-left|bottom-center|top-right

preferred_art: null       # ligne-claire|manga|realistic|ink-brush|chalk
preferred_tone: null      # neutral|warm|dramatic|romantic|energetic|vintage|action
preferred_layout: null    # standard|cinematic|dense|splash|mixed|webtoon
preferred_aspect: null    # 3:4|4:3|16:9

language: null            # zh|en|ja|ko|auto

character_presets:
  - name: my-characters
    roles:
      learner: "姓名"
      mentor: "姓名"
      challenge: "姓名"
      support: "姓名"
---
```

## 字段参考

| 字段 | 类型 | 默认值 | 说明 |
|-------|------|---------|-------------|
| `version` | int | 2 | Schema 版本 |
| `watermark.enabled` | bool | false | 启用水印 |
| `watermark.content` | string | "" | 水印文字（@用户名或自定义） |
| `watermark.position` | enum | bottom-right | 图像上的位置 |
| `preferred_art` | string | null | 画风（ligne-claire、manga、realistic、ink-brush、chalk） |
| `preferred_tone` | string | null | 基调（neutral、warm、dramatic、romantic、energetic、vintage、action） |
| `preferred_layout` | string | null | 布局偏好或 null |
| `preferred_aspect` | string | null | 宽高比（3:4、4:3、16:9） |
| `language` | string | null | 输出语言（null = 自动检测） |
| `character_presets` | array | [] | 预设角色配置，适用于 ohmsha 等风格 |

## 画风选项

| 值 | 中文 | 说明 |
|-------|------|-------------|
| `ligne-claire` | 清线 | 均匀线条，平涂色彩，欧洲漫画传统 |
| `manga` | 日漫 | 大眼睛，漫画惯例，情感表达丰富 |
| `realistic` | 写实 | 数字绘画，写实比例 |
| `ink-brush` | 水墨 | 中国毛笔笔触，水墨渲染效果 |
| `chalk` | 粉笔 | 黑板美学，手绘温暖感 |

## 基调选项

| 值 | 中文 | 说明 |
|-------|------|-------------|
| `neutral` | 中性 | 平衡、理性、教育性 |
| `warm` | 温馨 | 怀旧、私人、温暖 |
| `dramatic` | 戏剧 | 高对比、紧张、有力 |
| `romantic` | 浪漫 | 柔和、唯美、装饰性元素 |
| `energetic` | 活力 | 明亮、动感、激动人心 |
| `vintage` | 复古 | 历史感、陈旧、年代真实感 |
| `action` | 动作 | 速度线、冲击效果、格斗 |

## 位置选项

| 值 | 说明 |
|-------|-------------|
| `bottom-right` | 右下角（默认，适用于大多数分格布局） |
| `bottom-left` | 左下角 |
| `bottom-center` | 底部居中（适合 webtoon 竖版滚动） |
| `top-right` | 右上角（避免使用 - 与页码冲突） |

## 角色预设字段

| 字段 | 必需 | 说明 |
|-------|----------|-------------|
| `name` | 是 | 唯一的预设标识符 |
| `roles.learner` | 否 | 代表学习者/主角的角色 |
| `roles.mentor` | 否 | 代表老师/导师的角色 |
| `roles.challenge` | 否 | 代表障碍/反派的角色 |
| `roles.support` | 否 | 提供支持/搞笑的角色 |

## 示例：最小偏好

```yaml
---
version: 2
watermark:
  enabled: true
  content: "@myusername"
preferred_art: ligne-claire
preferred_tone: neutral
---
```

## 示例：完整偏好

```yaml
---
version: 2
watermark:
  enabled: true
  content: "@comicstudio"
  position: bottom-right

preferred_art: manga
preferred_tone: neutral

preferred_layout: webtoon

preferred_aspect: "3:4"

language: zh

character_presets:
  - name: tech-tutorial
    roles:
      learner: "小明"
      mentor: "教授"
      challenge: "难题怪"
      support: "小助手"
  - name: doraemon
    roles:
      learner: "大雄"
      mentor: "哆啦A梦"
      challenge: "胖虎"
      support: "静香"
---
```

## 从 v1 迁移

如果你有 v1 偏好文件中的 `preferred_style`，按以下方式迁移：

| 旧 `preferred_style.name` | 新 `preferred_art` | 新 `preferred_tone` |
|---------------------------|---------------------|---------------------|
| classic | ligne-claire | neutral |
| dramatic | ligne-claire | dramatic |
| warm | ligne-claire | warm |
| sepia | realistic | vintage |
| vibrant | manga | energetic |
| ohmsha | manga | neutral |
| realistic | realistic | neutral |
| wuxia | ink-brush | action |
| shoujo | manga | romantic |
| chalkboard | chalk | neutral |
