# Confirm Options (Cover Mode)

## Skip Conditions

| Condition | Skipped Questions | Still Asked |
|-----------|-------------------|-------------|
| `--quick` flag | Type, Palette, Rendering, Text, Mood, Font | **Aspect Ratio** (unless `--aspect`) |
| All 6 dimensions + `--aspect` specified | All | None |
| Otherwise | None | All questions |

## Quick Mode Output

When skipping 6 dimensions:

```
Quick Mode: Auto-selected dimensions
• Type: [type] ([reason])
• Palette: [palette] ([reason])
• Rendering: [rendering] ([reason])
• Text: [text] ([reason])
• Mood: [mood] ([reason])
• Font: [font] ([reason])

[Then ask Aspect Ratio if not specified]
```

## Confirmation Flow

**Language**: Auto-determined from user's input language > source language. No need to ask.

Present ALL options in a **single AskUserQuestion call** (up to 4 questions). Skip any question where the dimension is already specified via CLI flag or `--style` preset.

### Q1: Type (skip if `--type`)

```yaml
header: "Type"
question: "Which cover type?"
multiSelect: false
options:
  - label: "[auto-recommended type] (Recommended)"
    description: "[reason based on content signals]"
  - label: "hero"
    description: "Large visual impact, title overlay"
  - label: "conceptual"
    description: "Concept visualization, abstract shapes"
  - label: "typography"
    description: "Text-focused layout"
  - label: "metaphor"
    description: "Symbolic visual representing idea"
  - label: "scene"
    description: "Atmospheric environment"
  - label: "minimal"
    description: "Single element, generous whitespace"
```

### Q2: Palette (skip if `--palette` or `--style`)

```yaml
header: "Palette"
question: "Which color palette?"
multiSelect: false
options:
  - label: "[auto-recommended palette] (Recommended)"
    description: "[reason based on content signals]"
  - label: "warm"
    description: "Orange, golden yellow, terracotta"
  - label: "elegant"
    description: "Soft coral, muted teal, dusty rose"
  - label: "cool"
    description: "Engineering blue, navy, cyan"
  - label: "dark"
    description: "Charcoal, deep purple, electric accent"
  - label: "earth"
    description: "Forest green, warm brown, sage"
  - label: "vivid"
    description: "Electric blue, hot pink, lime"
  - label: "pastel"
    description: "Baby blue, blush pink, mint"
  - label: "mono"
    description: "Black, gray, white"
  - label: "retro"
    description: "Burnt sienna, mustard, avocado"
  - label: "duotone"
    description: "Two dominant colors, high contrast"
```

### Q3: Rendering (skip if `--rendering` or `--style`)

Show compatible renderings first (check compatibility matrix in auto-selection.md):

```yaml
header: "Rendering"
question: "Which rendering style?"
multiSelect: false
options:
  - label: "[best compatible rendering] (Recommended)"
    description: "[reason based on palette + type + content]"
  - label: "flat-vector"
    description: "Clean outlines, flat fills, geometric"
  - label: "hand-drawn"
    description: "Sketchy, organic, imperfect strokes"
  - label: "painterly"
    description: "Soft brush strokes, watercolor washes"
  - label: "digital"
    description: "Polished, precise, subtle gradients"
  - label: "pixel"
    description: "Retro pixel art, 16-bit aesthetic"
  - label: "chalk"
    description: "Chalkboard, rough textured strokes"
  - label: "screen-print"
    description: "Bold poster, halftone, silhouettes"
```

### Q4: Font + Settings

Combine remaining dimensions into one question:

```yaml
header: "Settings"
question: "Font / Text / Mood / Aspect?"
multiSelect: false
options:
  - label: "[auto-font] / [auto-text] / [auto-mood] / 16:9 (Recommended)"
    description: "Auto-selected: [font reason], [text reason], [mood reason]"
  - label: "[auto-font] / [auto-text] / bold / 16:9"
    description: "High contrast, vivid, dynamic"
  - label: "[auto-font] / [auto-text] / subtle / 16:9"
    description: "Low contrast, muted, calm"
  - label: "[auto-font] / [auto-text] / [auto-mood] / 1:1"
    description: "Square format for social media"
```

"Other" option (auto-added by AskUserQuestion) allows typing custom combo. Parse `/`-separated values.

## After Response

1. Log confirmed dimensions
2. Proceed to Step 4 (Prompt Construction)
