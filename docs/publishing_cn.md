# ClawHub / OpenClaw 发布

## OpenClaw 元数据

技能在 YAML front matter 中包含 `metadata.openclaw`：

```yaml
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#<skill-name>
    requires:          # 仅用于包含脚本的技能
      anyBins:
        - bun
        - npx
```

## 发布命令

```bash
bash scripts/sync-clawhub.sh           # 同步所有技能
bash scripts/sync-clawhub.sh <skill>   # 同步单个技能
```

发布钩子通过 `.releaserc.yml` 配置。本仓库不会创建单独的发布目录：发布准备阶段仅将 `packages/` 同步到每个技能已提交的 `scripts/vendor/` 中，发布时直接读取技能目录。

## 共享工作区包

`packages/` 是**唯一**的真实来源。请勿直接编辑 `skills/*/scripts/vendor/`。

当前包：
- `baoyu-chrome-cdp`（Chrome CDP 工具），被 6 个技能使用（`baoyu-danger-gemini-web`、`baoyu-danger-x-to-markdown`、`baoyu-post-to-wechat`、`baoyu-post-to-weibo`、`baoyu-post-to-x`、`baoyu-url-to-markdown`）
- `baoyu-md`（共享 Markdown 渲染和占位符处理管道），被 3 个技能使用（`baoyu-markdown-to-html`、`baoyu-post-to-wechat`、`baoyu-post-to-weibo`）

**工作原理**：同步脚本将包复制到每个使用该包的技能的 `vendor/` 目录中，并将依赖规格重写为 `file:./vendor/<name>`。Vendor 副本提交到 Git，使技能保持自包含。

**更新工作流**：
1. 在 `packages/` 下编辑包
2. 运行 `node scripts/sync-shared-skill-packages.mjs`
3. 将同步后的 `vendor/`、`package.json` 和 `bun.lock` 一起提交

**Git 钩子**：运行 `node scripts/install-git-hooks.mjs` 一次以启用 `pre-push` 钩子。它会重新同步并在 vendor 副本过时时阻止推送（`--enforce-clean`）。
