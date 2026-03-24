# 小红书大纲模板

用于生成带版式说明的信息图系列大纲。

## 文件命名

大纲文件名中包含策略标识：
- `outline-strategy-a.md` — 故事驱动变体
- `outline-strategy-b.md` — 信息密集变体
- `outline-strategy-c.md` — 视觉优先变体
- `outline.md` — 最终选用版（从选定变体复制）

## 图片文件命名

图片使用有意义的 slug 便于阅读：
```
NN-{type}-[slug].png
NN-{type}-[slug].md（位于 prompts/）
```

| 类型 | 用途 |
|------|-------|
| `cover` | 首张（封面） |
| `content` | 中间内容图 |
| `ending` | 最后一张 |

**示例**：
- `01-cover-ai-tools.png`
- `02-content-why-ai.png`
- `03-content-chatgpt.png`
- `04-content-midjourney.png`
- `05-content-notion-ai.png`
- `06-ending-summary.png`

**Slug 规则**：
- 由画面内容派生（kebab-case）
- 系列内需唯一
- 短而有意（约 2–4 个词）

## 版式选择指南

### 按密度

| 版式 | 何时使用 | 信息点数量 | 留白比例 |
|--------|-------------|-------------|------------|
| sparse | 封面、金句、强冲击单句 | 1-2 | 60-70% |
| balanced | 常规内容、教程 | 3-4 | 40-50% |
| dense | 知识卡、速查 | 5-8 | 20-30% |

### 按结构

| 版式 | 何时使用 | 结构 |
|--------|-------------|-----------|
| list | 排行、清单、步骤 | 纵向编号/项目符号 |
| comparison | Before/after、利弊 | 左右分屏 |
| flow | 流程、时间线 | 箭头连接的节点 |

### 按页面位置的建议

| 位置 | 推荐 | 理由 |
|----------|-------------|-----------|
| 封面 | sparse | 冲击力强、标题清晰 |
| 铺垫 | balanced | 交代背景但不拥挤 |
| 核心 | balanced/dense/list | 与信息密度匹配 |
| 收获 | balanced/list | 要点清楚 |
| 结尾 | sparse | CTA 干净、易记住 |

## 大纲格式

```markdown
# 小红书信息图系列大纲

---
strategy: a  # a、b 或 c
name: Story-Driven
style: notion
default_layout: dense
image_count: 6
generated: YYYY-MM-DD HH:mm
---

## 第 1 张 / 共 6 张

**位置**: Cover
**版式**: sparse
**钩子**: 打工人必看！
**Slug**: ai-tools
**文件名**: 01-cover-ai-tools.png

**文案内容**:
- 标题: 「5个AI神器让你效率翻倍」
- 副标题: 亲测好用，建议收藏

**画面概念**:
科技感背景，多个AI工具图标环绕，中心大标题，
霓虹蓝+深色背景，未来感十足

**划动钩子**: 第一个就很强大👇

---

## 第 2 张 / 共 6 张

**位置**: Content
**版式**: balanced
**核心信息**: 为什么你需要AI工具
**Slug**: why-ai
**文件名**: 02-content-why-ai.png

**文案内容**:
- 标题: 「为什么要用AI？」
- 要点:
  - 重复工作自动化
  - 创意辅助不卡壳
  - 效率提升10倍

**画面概念**:
对比图：左边疲惫打工人，右边轻松使用AI的人
科技线条装饰，简洁有力

**划动钩子**: 接下来是具体工具推荐👇

---

## 第 3 张 / 共 6 张

**位置**: Content
**版式**: dense
**核心信息**: ChatGPT使用技巧
**Slug**: chatgpt
**文件名**: 03-content-chatgpt.png

**文案内容**:
- 标题: 「ChatGPT」
- 副标题: 最强AI助手
- 要点:
  - 写文案：给出框架，秒出初稿
  - 改文章：润色、翻译、总结
  - 编程：写代码、找bug
  - 学习：解释概念、出题练习

**画面概念**:
ChatGPT logo居中，四周放射状展示功能点
深色科技背景，霓虹绿点缀

**划动钩子**: 下一个更适合创意工作者👇

---

## 第 4 张 / 共 6 张

**位置**: Content
**版式**: dense
**核心信息**: Midjourney绘图
**Slug**: midjourney
**文件名**: 04-content-midjourney.png

**文案内容**:
- 标题: 「Midjourney」
- 副标题: AI绘画神器
- 要点:
  - 输入描述，秒出图片
  - 风格多样：写实/插画/3D
  - 做封面、做头像、做素材
  - 不会画画也能当设计师

**画面概念**:
展示几张MJ生成的不同风格图片
画框/画布元素装饰

**划动钩子**: 还有一个效率神器👇

---

## 第 5 张 / 共 6 张

**位置**: Content
**版式**: balanced
**核心信息**: Notion AI笔记
**Slug**: notion-ai
**文件名**: 05-content-notion-ai.png

**文案内容**:
- 标题: 「Notion AI」
- 副标题: 智能笔记助手
- 要点:
  - 自动总结长文
  - 头脑风暴出点子
  - 整理会议记录

**画面概念**:
Notion界面风格，简洁黑白配色
展示笔记整理前后对比

**划动钩子**: 最后总结一下👇

---

## 第 6 张 / 共 6 张

**位置**: Ending
**版式**: sparse
**核心信息**: 总结与互动
**Slug**: summary
**文件名**: 06-ending-summary.png

**文案内容**:
- 标题: 「工具只是工具」
- 副标题: 关键是用起来！
- CTA: 收藏备用 | 转发给需要的朋友
- 互动: 你最常用哪个？评论区见👇

**画面概念**:
简洁背景，大字标题
底部互动引导文字
收藏/分享图标

---
```

## 划动钩子策略

每张图结尾应为下一张留钩子：

| 策略 | 示例 |
|----------|---------|
| 预告 | "第一个就很强大👇" |
| 编号 | "接下来是第2个👇" |
| 最高级 | "下一个更厉害👇" |
| 提问 | "猜猜下一个是什么？👇" |
| 承诺 | "最后一个最实用👇" |
| 紧迫感 | "最重要的来了👇" |

## 策略区分

三种策略应有明显差异：

| 策略 | 重点 | 结构 | 张数 |
|----------|-------|-----------|------------|
| A: 故事驱动 | 情绪、个人化 | Hook→Problem→Discovery→Experience→Conclusion | 4-6 |
| B: 信息密集 | 事实、结构化 | Core→Info Cards→Comparison→Recommendation | 3-5 |
| C: 视觉优先 | 氛围、少字 | Hero→Details→Lifestyle→CTA | 3-4 |

**「AI工具推荐」示例**：
- `outline-strategy-a.md`: Warm + Balanced — 个人使用 AI 的叙事线
- `outline-strategy-b.md`: Notion + Dense — 知识卡片风格
- `outline-strategy-c.md`: Minimal + Sparse — 利落科技美学
