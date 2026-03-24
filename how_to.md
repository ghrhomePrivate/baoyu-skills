现在我完整看明白了整个机制。回答你的三个问题：

## 1. 用户项目中不需要这样配置

根目录的 `package.json` 中的 `workspaces` 配置是**这个插件仓库自身开发维护用的**，不是给安装 skill 的用户用的。用户安装 skill 后不需要也不会接触到这个配置。

## 2. 共享机制：既不是纯 npm workspaces，也不是 skill 定义，而是自定义同步脚本

这是这个项目最巧妙的设计。整个流程分两层：

**开发时（源码层）**：`packages/` 是共享库的唯一源头

```
packages/
├── baoyu-md/          ← Markdown 渲染库（源码）
└── baoyu-chrome-cdp/  ← Chrome CDP 库（源码）
```

**分发时（vendor 层）**：通过 `scripts/sync-shared-skill-packages.mjs` 这个自定义脚本，把 `packages/` 下的源码**复制**到每个需要它的 skill 的 `scripts/vendor/` 目录中。

具体流程如下：

```
sync-shared-skill-packages.mjs 执行时：

1. 扫描 packages/ 下的所有 workspace 包（发现 baoyu-md、baoyu-chrome-cdp）
2. 扫描 skills/*/scripts/package.json，找到依赖了这些包的 skill
3. 把源码整个复制到 skill 的 scripts/vendor/ 目录
4. 把 skill 的 package.json 中的依赖路径改写为 file:./vendor/xxx
5. 运行 bun install 刷新依赖
```

所以你会看到每个 skill 里都有这样的结构：

```
skills/baoyu-markdown-to-html/scripts/
├── package.json          ← "baoyu-md": "file:./vendor/baoyu-md"
├── main.ts
└── vendor/
    └── baoyu-md/         ← 从 packages/baoyu-md 复制过来的完整副本
        ├── package.json
        └── src/
            └── index.ts
```

```6:6:skills/baoyu-markdown-to-html/scripts/package.json
    "baoyu-md": "file:./vendor/baoyu-md"
```

## 3. 为什么要这样设计？

**目的是让每个 skill 自包含（self-contained）**。

| 层面 | 机制 | 用途 |
|------|------|------|
| `packages/` + root `workspaces` | npm workspaces | 开发时方便统一修改、跑测试 |
| `sync-shared-skill-packages.mjs` | 自定义脚本 | 把共享包复制到各 skill 的 vendor 里 |
| `file:./vendor/xxx` | npm 本地路径依赖 | 让 skill 脱离 workspace 也能独立运行 |

这样设计的好处是：
- **开发者**只需维护 `packages/` 下的一份源码，修改后跑一次 sync 就全部更新
- **skill 分发后**不依赖根目录的 workspace 配置，每个 skill 目录自带所有依赖，可以独立执行
- 用户安装插件后，每个 skill 直接用自己 `vendor/` 里的代码，不需要任何额外配置

简单说：**`workspaces` 是给开发者用的便利工具，`vendor` 复制是给运行时用的分发策略，两者通过 sync 脚本桥接。**