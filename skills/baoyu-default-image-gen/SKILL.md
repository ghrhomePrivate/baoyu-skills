---
name: baoyu-default-image-gen
description: Zero-config image generation using the IDE's built-in image capability (e.g., Cursor GenerateImage). No API key or external tools required. Supports 5-dimensional cover customization (type, palette, rendering, text, mood), article illustrations, and general image generation. Use as fallback when baoyu-image-gen is not configured, or when user prefers IDE-native generation.
version: 1.0.0
---

# Default Image Generation (IDE-Native)

Generate images using the IDE's built-in image generation tool (e.g., Cursor's `GenerateImage`). Zero configuration — no API keys, no Bun, no EXTEND.md required.

## When to Use This Skill

| Scenario | Use this skill |
|----------|---------------|
| No `baoyu-image-gen` EXTEND.md configured | ✓ |
| No API keys available | ✓ |
| Bun/npx not installed | ✓ |
| User explicitly requests IDE-native generation | ✓ |
| Need reference image editing (`--ref`) | ✗ Use baoyu-image-gen |
| Need batch generation (`--batchfile`) | ✗ Use baoyu-image-gen |

## Usage

```bash
# Cover image with auto-selection
/baoyu-default-image-gen cover article.md

# Cover with specific dimensions
/baoyu-default-image-gen cover article.md --type conceptual --palette cool --rendering flat-vector

# Quick mode: skip confirmation
/baoyu-default-image-gen cover article.md --quick

# Style preset
/baoyu-default-image-gen cover article.md --style blueprint

# General image from prompt
/baoyu-default-image-gen image --prompt "A futuristic city skyline at sunset"

# Article illustration
/baoyu-default-image-gen illustration article.md --position "after section 2"
```

## Modes

| Mode | Description | Workflow |
|------|-------------|----------|
| `cover` | Article cover image with 5-dimensional customization | Full confirm flow |
| `illustration` | Article illustration at specific position | Content-driven prompt |
| `image` | General image from text prompt | Direct generation |

## Cover Mode Options

| Option | Description |
|--------|-------------|
| `--type <name>` | hero, conceptual, typography, metaphor, scene, minimal |
| `--palette <name>` | warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone |
| `--rendering <name>` | flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print |
| `--style <name>` | Preset shorthand (sets palette + rendering) |
| `--text <level>` | none, title-only, title-subtitle, text-rich |
| `--mood <level>` | subtle, balanced, bold |
| `--font <name>` | clean, handwritten, serif, display |
| `--aspect <ratio>` | 16:9 (default), 2.35:1, 4:3, 3:2, 1:1, 3:4 |
| `--lang <code>` | Title language (en, zh, ja, etc.) |
| `--no-title` | Alias for `--text none` |
| `--quick` | Skip confirmation, use auto-selection |

## Five Dimensions (Cover Mode)

| Dimension | Values | Default |
|-----------|--------|---------|
| **Type** | hero, conceptual, typography, metaphor, scene, minimal | auto |
| **Palette** | warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone | auto |
| **Rendering** | flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print | auto |
| **Text** | none, title-only, title-subtitle, text-rich | title-only |
| **Mood** | subtle, balanced, bold | balanced |
| **Font** | clean, handwritten, serif, display | clean |

Auto-selection rules: [references/auto-selection.md](references/auto-selection.md)

## Style Presets

| Preset | Palette | Rendering | Best For |
|--------|---------|-----------|----------|
| `blueprint` | cool | flat-vector | Technical, architecture |
| `editorial` | elegant | hand-drawn | Stories, thought pieces |
| `neon` | dark | digital | Entertainment, gaming |
| `nature` | earth | painterly | Wellness, travel |
| `retro-print` | retro | screen-print | Culture, history |
| `minimal-ink` | mono | hand-drawn | Zen, simplicity |
| `pixel-pop` | vivid | pixel | Gaming, retro |
| `chalk-edu` | pastel | chalk | Education, tutorial |
| `warm-sketch` | warm | hand-drawn | Personal, lifestyle |
| `duotone-poster` | duotone | screen-print | Movie, concert |

## Workflow

### Progress Checklist

```
Default Image Gen Progress:
- [ ] Step 1: Determine mode (cover / illustration / image)
- [ ] Step 2: Analyze content (cover/illustration only)
- [ ] Step 3: Confirm options ⚠️ (cover mode, unless --quick)
- [ ] Step 4: Construct prompt description
- [ ] Step 5: Generate image via IDE tool
- [ ] Step 6: Save and report
```

### Step 1: Determine Mode

| Input | Mode |
|-------|------|
| `cover` keyword + article/content | Cover mode → full 5-dimension flow |
| `illustration` keyword + article | Illustration mode → content-driven |
| `image` keyword + prompt text | Image mode → direct generation |
| Article file without keyword | Default to Cover mode |
| Plain text prompt without keyword | Default to Image mode |

### Step 2: Analyze Content (Cover / Illustration)

1. Read article content (file path or pasted text)
2. Extract: topic, tone, keywords, visual metaphors
3. Detect language from content
4. Determine recommended dimensions using auto-selection rules

### Step 3: Confirm Options ⚠️ (Cover Mode)

**Skip conditions**:

| Condition | Skipped | Still Asked |
|-----------|---------|-------------|
| `--quick` | 6 dimensions | Aspect ratio (unless `--aspect`) |
| All dimensions specified | All | None |

**MUST use `AskUserQuestion`** to present options interactively. Full flow: [references/workflow/confirm-options.md](references/workflow/confirm-options.md)

Present up to 4 questions in a single `AskUserQuestion` call:
- Q1: Type
- Q2: Palette
- Q3: Rendering
- Q4: Font + Settings (text, mood, aspect combined)

Each question shows the auto-recommended option first with reason, followed by alternatives.

### Step 4: Construct Prompt Description

Build a detailed image description based on confirmed dimensions. This description is passed to the IDE's image generation tool.

Prompt construction guide: [references/prompt-construction.md](references/prompt-construction.md)

**Cover prompt structure**:

```
[Scene/composition description based on type]
[Visual metaphor derived from article content]
[Color scheme from palette definition, adjusted by mood]
[Rendering style characteristics]
[Text elements if applicable]
[Aspect ratio and layout notes]
```

**Illustration prompt structure**:

```
[Scene description based on article section context]
[Visual style matching article tone]
[Key concepts visualized from surrounding text]
```

### Step 5: Generate Image via IDE Tool

Use the IDE's built-in image generation capability:

- **Cursor**: Call `GenerateImage` tool with the constructed description
- **Other IDEs**: Use whatever native image generation is available

**Parameters**:
- `description`: The full prompt from Step 4
- `filename`: Output filename (e.g., `cover.png`, `illustration-01.png`)

**IMPORTANT**: The IDE tool does NOT support `--ref` (reference images). If user needs reference-based generation, suggest switching to `baoyu-image-gen`.

### Step 6: Save and Report

**Cover mode output**:

```
<output-dir>/
├── prompts/cover.md       # Saved prompt for reproducibility
└── cover.png              # Generated image
```

**Illustration mode output**:

```
<output-dir>/
├── prompts/illustration-NN.md
└── illustration-NN.png
```

**Completion report**:

```
Image Generated!

Mode: [cover/illustration/image]
[Cover only] Type: [type] | Palette: [palette] | Rendering: [rendering]
[Cover only] Text: [text] | Mood: [mood] | Font: [font]
Aspect: [ratio]
Language: [lang]
Location: [output path]

Files:
✓ prompts/[name].md
✓ [name].png
```

## Image Modification

| Action | Steps |
|--------|-------|
| **Regenerate** | Update prompt → Regenerate with IDE tool |
| **Change dimension** | Confirm new value → Update prompt → Regenerate |
| **Switch to API backend** | Configure baoyu-image-gen EXTEND.md + API key |

## Composition Principles

- **Whitespace**: 40-60% breathing room
- **Visual anchor**: Main element centered or offset left
- **Characters**: Simplified silhouettes; NO realistic humans
- **Title**: Use exact title from user/source; never invent
- **Consistency**: When generating multiple images, maintain style coherence by reusing the same dimension settings

## Limitations vs baoyu-image-gen

| Feature | This skill | baoyu-image-gen |
|---------|-----------|-----------------|
| API key required | No | Yes |
| Setup required | None | EXTEND.md + .env |
| Reference images (`--ref`) | Not supported | Supported |
| Batch generation | Not supported | Supported |
| Provider choice | IDE default | 7 providers |
| Image quality control | IDE default | normal / 2k / 4k |
| Custom image size | Not supported | Supported |

## Interop with Other Skills

This skill can be used as a drop-in replacement for `baoyu-image-gen` in the generation step of:

- `baoyu-cover-image` (Step 4: Generate Image)
- `baoyu-article-illustrator` (image generation step)
- `baoyu-slide-deck` (slide image generation)
- `baoyu-infographic` (infographic generation)
- `baoyu-comic` (panel generation)
- `baoyu-xhs-images` (card generation)

When another skill checks available image generation backends, this skill should be listed if no `baoyu-image-gen` EXTEND.md is configured.

## References

**Auto-Selection**: [references/auto-selection.md](references/auto-selection.md)
**Prompt Construction**: [references/prompt-construction.md](references/prompt-construction.md)
**Confirm Options**: [references/workflow/confirm-options.md](references/workflow/confirm-options.md)
