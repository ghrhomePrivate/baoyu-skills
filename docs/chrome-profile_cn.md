# Chrome 配置文件

所有 CDP 技能共享同一个配置文件目录。请勿为每个技能创建单独的配置文件。

覆盖：`BAOYU_CHROME_PROFILE_DIR` 环境变量（优先级高于所有默认值）。

| 平台 | 默认路径 |
|------|---------|
| macOS | `~/Library/Application Support/baoyu-skills/chrome-profile` |
| Linux | `$XDG_DATA_HOME/baoyu-skills/chrome-profile`（回退到 `~/.local/share/`） |
| Windows | `%APPDATA%/baoyu-skills/chrome-profile` |
| WSL | Windows 主目录 `/.local/share/baoyu-skills/chrome-profile` |

新技能：仅使用 `BAOYU_CHROME_PROFILE_DIR`（不使用每个技能独有的环境变量如 `X_BROWSER_PROFILE_DIR`）。

## 实现模式

```typescript
function getDefaultProfileDir(): string {
  const override = process.env.BAOYU_CHROME_PROFILE_DIR?.trim();
  if (override) return path.resolve(override);
  const base = process.platform === 'darwin'
    ? path.join(os.homedir(), 'Library', 'Application Support')
    : process.env.XDG_DATA_HOME || path.join(os.homedir(), '.local', 'share');
  return path.join(base, 'baoyu-skills', 'chrome-profile');
}
```
