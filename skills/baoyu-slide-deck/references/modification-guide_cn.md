# 幻灯片修改指南

初始生成后修改单张幻灯片的工作流程。

## 编辑单张幻灯片

用修改后的内容重新生成特定幻灯片：

1. 确定要编辑的幻灯片（如 `03-slide-key-findings.png`）
2. 更新 `prompts/03-slide-key-findings.md` 中的提示词
3. 如果内容变化较大，更新文件名中的 slug
4. 使用相同的会话 ID 重新生成图像
5. 重新生成 PPTX 和 PDF

## 添加新幻灯片

在指定位置插入新幻灯片：

1. 指定插入位置（如第 3 张幻灯片之后）
2. 创建带有合适 slug 的新提示词（如 `04-slide-new-section.md`）
3. 生成新幻灯片图像
4. **重编文件序号**：所有后续幻灯片的 NN 加 1
   - `04-slide-conclusion.png` → `05-slide-conclusion.png`
   - Slug 保持不变
5. 更新 `outline.md` 中的新幻灯片条目
6. 重新生成 PPTX 和 PDF

## 删除幻灯片

删除幻灯片并重编序号：

1. 确定要删除的幻灯片（如 `03-slide-key-findings.png`）
2. 删除图像文件和提示词文件
3. **重编文件序号**：所有后续幻灯片的 NN 减 1
   - `04-slide-conclusion.png` → `03-slide-conclusion.png`
   - Slug 保持不变
4. 更新 `outline.md` 删除对应幻灯片条目
5. 重新生成 PPTX 和 PDF

## 文件命名约定

文件使用有意义的 slug 提高可读性：

```
NN-slide-[slug].png
NN-slide-[slug].md (在 prompts/ 中)
```

示例：
- `01-slide-cover.png`
- `02-slide-problem-statement.png`
- `03-slide-key-findings.png`
- `04-slide-back-cover.png`

## Slug 规则

| 规则 | 描述 |
|------|-------------|
| 格式 | Kebab-case（小写、连字符） |
| 来源 | 从幻灯片标题/内容派生 |
| 唯一性 | 在幻灯片组内必须唯一 |
| 更新 | 内容显著变化时更改 slug |

## 重编序号规则

| 场景 | 操作 |
|----------|--------|
| 添加幻灯片 | 所有后续幻灯片 NN 加 1 |
| 删除幻灯片 | 所有后续幻灯片 NN 减 1 |
| 重排幻灯片 | 更新 NN 匹配新位置 |
| 编辑幻灯片 | NN 不变，如需要更新 slug |

**重要**：重编序号时 slug 保持不变。仅 NN 前缀变化。

## 修改后检查清单

任何修改后：

- [ ] 图像文件正确重命名/创建
- [ ] 提示词文件正确重命名/创建
- [ ] 后续文件已重编序号（如添加/删除）
- [ ] `outline.md` 已更新以反映变化
- [ ] PPTX 已重新生成
- [ ] PDF 已重新生成
- [ ] 大纲头部中的幻灯片数量已更新
