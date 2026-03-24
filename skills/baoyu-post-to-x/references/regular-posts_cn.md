# 普通发帖 — 详细指南

向 X 发布文字与图片的详细说明。

## 手动流程

若希望逐步自行控制：

### 步骤 1：将图片复制到剪贴板

```bash
${BUN_X} {baseDir}/scripts/copy-to-clipboard.ts image /path/to/image.png
```

### 步骤 2：从剪贴板粘贴

```bash
# 粘贴到最前应用
${BUN_X} {baseDir}/scripts/paste-from-clipboard.ts

# 带重试地粘贴到 Chrome
${BUN_X} {baseDir}/scripts/paste-from-clipboard.ts --app "Google Chrome" --retries 5

# 更短延迟的快速粘贴
${BUN_X} {baseDir}/scripts/paste-from-clipboard.ts --delay 200
```

### 步骤 3：使用 Playwright MCP（若已有 Chrome 会话）

```bash
# Navigate
mcp__playwright__browser_navigate url="https://x.com/compose/post"

# Get element refs
mcp__playwright__browser_snapshot

# Type text
mcp__playwright__browser_click element="editor" ref="<ref>"
mcp__playwright__browser_type element="editor" ref="<ref>" text="Your content"

# Paste image (after copying to clipboard)
mcp__playwright__browser_press_key key="Meta+v"  # macOS
# or
mcp__playwright__browser_press_key key="Control+v"  # Windows/Linux

# Screenshot to verify
mcp__playwright__browser_take_screenshot filename="preview.png"
```

## 图片支持

- 格式：PNG、JPEG、GIF、WebP
- 每条帖最多 4 张图
- 图片先复制到系统剪贴板，再通过快捷键粘贴

## 示例会话

```
User: /post-to-x "Hello from Claude!" --image ./screenshot.png

Claude:
1. 执行：${BUN_X} {baseDir}/scripts/x-browser.ts "Hello from Claude!" --image ./screenshot.png
2. Chrome 打开 X 发帖页
3. 在编辑器中输入文字
4. 图片复制到剪贴板并粘贴
5. 浏览器保持约 30 秒供预览
6. 汇报：「帖子已填好。使用 --submit 发布。」
```

## 故障排除

- **找不到 Chrome**：设置环境变量 `X_BROWSER_CHROME_PATH`
- **未登录**：首次运行会打开 Chrome，需手动登录，Cookie 会保存
- **图片粘贴失败**：
  - 校验剪贴板脚本：`${BUN_X} {baseDir}/scripts/copy-to-clipboard.ts image <path>`
  - macOS：在 系统设置 → 隐私与安全性 → 辅助功能 中为 Terminal/iTerm 授权
  - 粘贴时保持 Chrome 窗口可见并置于前台
- **osascript 权限被拒绝**：在系统偏好设置中为终端授予辅助功能权限
- **触发限流**：等待数分钟后再试

## 实现原理

`x-browser.ts` 使用 Chrome DevTools Protocol（CDP）：
1. 启动真实 Chrome（非 Playwright），带 `--disable-blink-features=AutomationControlled`
2. 使用持久 profile 保存登录状态
3. 通过 CDP（`Runtime.evaluate`、`Input.dispatchKeyEvent`）与 X 交互
4. **macOS 下用 osascript 粘贴图片**：向 Chrome 发送真实 Cmd+V，绕过 CDP 合成事件（X 可检测）

从而绕过 X 对 Playwright/Puppeteer 的反自动化策略。

### macOS 图片粘贴机制

CDP 的 `Input.dispatchKeyEvent` 发送的是「合成」键盘事件，站点可检测。X 会忽略合成粘贴。做法是：

1. 通过 Swift/AppKit 将图片写入系统剪贴板（`copy-to-clipboard.ts`）
2. 用 `osascript` 将 Chrome 置前
3. 通过 `osascript` 与 System Events 发送真实 Cmd+V
4. 等待上传完成

这需要终端在系统设置中拥有「辅助功能」权限。
