# 风格预设（Style Presets）

`--preset X` 会展开为「风格 + 版式」组合。用户可分别覆盖其中任一维度。

| 预设（`--preset`） | 风格（`style`） | 版式（`layout`） |
|------------------|----------------|----------------|
| `knowledge-card` | `notion` | `dense` |
| `checklist` | `notion` | `list` |
| `concept-map` | `notion` | `mindmap` |
| `swot` | `notion` | `quadrant` |
| `tutorial` | `chalkboard` | `flow` |
| `classroom` | `chalkboard` | `balanced` |
| `study-guide` | `study-notes` | `dense` |
| `cute-share` | `cute` | `balanced` |
| `girly` | `cute` | `sparse` |
| `cozy-story` | `warm` | `balanced` |
| `product-review` | `fresh` | `comparison` |
| `nature-flow` | `fresh` | `flow` |
| `warning` | `bold` | `list` |
| `versus` | `bold` | `comparison` |
| `clean-quote` | `minimal` | `sparse` |
| `pro-summary` | `minimal` | `balanced` |
| `retro-ranking` | `retro` | `list` |
| `throwback` | `retro` | `balanced` |
| `pop-facts` | `pop` | `list` |
| `hype` | `pop` | `sparse` |
| `poster` | `screen-print` | `sparse` |
| `editorial` | `screen-print` | `balanced` |
| `cinematic` | `screen-print` | `comparison` |

## 覆盖示例

- `--preset knowledge-card --style chalkboard` = chalkboard 风格 + dense 版式
- `--preset poster --layout quadrant` = screen-print 风格 + quadrant 版式

显式传入 `--style` / `--layout` 时，始终覆盖 preset 中的对应值。
