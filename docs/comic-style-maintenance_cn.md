# 风格维护（baoyu-comic）

## 添加新风格

1. 创建风格定义文件：`skills/baoyu-comic/references/styles/<style-name>.md`
2. 更新 SKILL.md：添加到 `--style` 选项表 + 自动选择条目
3. 生成展示图片：
   ```bash
   ${BUN_X} skills/baoyu-danger-gemini-web/scripts/main.ts \
     --prompt "A single comic book page in <style-name> style showing [scene]. Features: [characteristics]. 3:4 portrait aspect ratio comic page." \
     --image screenshots/comic-styles/<style-name>.png
   ```
4. 压缩：`${BUN_X} skills/baoyu-compress-image/scripts/main.ts screenshots/comic-styles/<style-name>.png`
5. 更新两个 README 文件（`README.md` + `README.zh.md`）：添加风格到选项、描述表格和预览网格中

## 更新现有风格

1. 更新 `references/styles/` 中的风格定义
2. 如果视觉特征发生变化，重新生成展示图片（上述步骤 3-4）
3. 如果描述发生变化，更新 README 文件

## 删除风格

1. 删除风格定义文件和展示图片（`.webp`）
2. 从 SKILL.md 的 `--style` 选项和自动选择中移除
3. 从两个 README 文件中移除（选项、描述表格、预览网格）

## 风格预览网格格式

```markdown
| | | |
|:---:|:---:|:---:|
| ![style1](./screenshots/comic-styles/style1.webp) | ![style2](./screenshots/comic-styles/style2.webp) | ![style3](./screenshots/comic-styles/style3.webp) |
| style1 | style2 | style3 |
```
