# 创建新技能

**必读**：[技能编写最佳实践](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

## 关键要求

| 要求 | 详情 |
|------|------|
| **前缀** | 所有技能必须使用 `baoyu-` 前缀 |
| **name 字段** | 最多 64 个字符，仅限小写字母/数字/连字符，不得包含 "anthropic"/"claude" |
| **description** | 最多 1024 个字符，使用第三人称，包含功能和使用场景 |
| **SKILL.md 正文** | 控制在 500 行以内；使用 `references/` 存放额外内容 |
| **引用文件** | 从 SKILL.md 只嵌套一层；避免多层嵌套引用 |

## SKILL.md Front Matter 模板

```yaml
---
name: baoyu-<name>
description: <第三人称描述。功能 + 使用场景。>
version: <与 marketplace.json 一致的语义化版本号>
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-<name>
    requires:          # 仅在技能包含脚本时添加
      anyBins:
        - bun
        - npx
---
```

## 步骤

1. 创建 `skills/baoyu-<name>/SKILL.md` 并添加 YAML front matter
2. 在 `skills/baoyu-<name>/scripts/` 中添加 TypeScript 脚本（如适用）
3. 如需要，在 `skills/baoyu-<name>/prompts/` 中添加提示词模板
4. 在 `marketplace.json` 中注册到适当的类别
5. 如果技能包含脚本，在 SKILL.md 中添加脚本目录（Script Directory）部分
6. 在 front matter 中添加 openclaw 元数据

## 类别选择

| 如果你的技能... | 使用类别 |
|----------------|---------|
| 生成视觉内容（图片、幻灯片、漫画） | `content-skills` |
| 发布到平台（X、微信、微博） | `content-skills` |
| 提供 AI 生成后端 | `ai-generation-skills` |
| 转换或处理内容 | `utility-skills` |

新类别：在 `marketplace.json` 中添加包含 `name`、`description`、`skills[]` 的插件对象。

## 编写描述

**必须使用第三人称**：

```yaml
# 正确
description: Generates Xiaohongshu infographic series from content. Use when user asks for "小红书图片", "XHS images".

# 错误
description: I can help you create Xiaohongshu images
```

## 脚本目录模板

每个包含脚本的 SKILL.md 必须包含：

```markdown
## Script Directory

**Important**: All scripts are located in the `scripts/` subdirectory of this skill.

**Agent Execution Instructions**:
1. Determine this SKILL.md file's directory path as `{baseDir}`
2. Script path = `{baseDir}/scripts/<script-name>.ts`
3. Resolve `${BUN_X}` runtime: if `bun` installed → `bun`; if `npx` available → `npx -y bun`; else suggest installing bun
4. Replace all `{baseDir}` and `${BUN_X}` in this document with actual values

**Script Reference**:
| Script | Purpose |
|--------|---------|
| `scripts/main.ts` | Main entry point |
```

## 渐进式披露

对于内容较多的技能：

```
skills/baoyu-example/
├── SKILL.md              # 主要说明（<500 行）
├── references/
│   ├── styles.md         # 按需加载
│   └── examples.md       # 按需加载
└── scripts/
    └── main.ts
```

从 SKILL.md 链接（仅一层深度）：
```markdown
**Available styles**: See [references/styles.md](references/styles.md)
```

## 扩展支持（EXTEND.md）

每个 SKILL.md 必须包含 EXTEND.md 加载逻辑。在工作流技能中作为步骤 1.1 添加，在工具技能中作为"偏好设置"部分添加：

```markdown
**1.1 Load Preferences (EXTEND.md)**

Check EXTEND.md existence (priority order):

\`\`\`bash
test -f .baoyu-skills/<skill-name>/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/<skill-name>/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/<skill-name>/EXTEND.md" && echo "user"
\`\`\`

| 路径 | 位置 |
|------|------|
| `.baoyu-skills/<skill-name>/EXTEND.md` | 项目目录 |
| `$XDG_CONFIG_HOME/baoyu-skills/<skill-name>/EXTEND.md` | XDG 配置目录（~/.config） |
| `$HOME/.baoyu-skills/<skill-name>/EXTEND.md` | 用户主目录（旧版） |

| 结果 | 操作 |
|------|------|
| 找到 | 读取、解析、显示摘要 |
| 未找到 | 使用 AskUserQuestion 询问用户 |
```

SKILL.md 末尾应包含：
```markdown
## Extension Support
Custom configurations via EXTEND.md. See **Step 1.1** for paths and supported options.
```
