# Prompt Construction Guide

How to build detailed image descriptions from confirmed dimensions for IDE-native image generation.

## Core Principle

IDE image generation tools (like Cursor's GenerateImage) accept a **text description** and return an image. The description must be self-contained: specific, concrete, and detailed enough to produce a consistent result without reference images.

## Cover Image Prompt Template

```
A [aspect_ratio] cover image for an article about [topic].

Style: [rendering_description]. [mood_description].
Color scheme: [palette_colors].
Composition: [type_composition].

Visual elements: [content_metaphors_and_icons].
[text_elements_if_applicable].

[rendering_notes]. [palette_notes].
No realistic human faces. Clean, professional composition with 40-60% whitespace.
```

## Dimension → Description Mapping

### Type → Composition

| Type | Description Fragment |
|------|---------------------|
| `hero` | "Large focal visual occupying 60-70% of the image area. Dramatic composition with the main element as a bold centerpiece. Title overlaid on the visual." |
| `conceptual` | "Abstract shapes and geometric forms representing core concepts. Clean information hierarchy with distinct visual zones. Connected elements suggesting relationships." |
| `typography` | "Text as the primary visual element occupying 40%+ of the image area. Minimal supporting visuals. Strong typographic hierarchy with the title dominating the composition." |
| `metaphor` | "A concrete object or scene representing an abstract idea. Symbolic elements creating emotional resonance. The metaphor as visual anchor with supporting contextual elements." |
| `scene` | "An atmospheric environment with narrative elements. Mood-setting lighting and immersive spatial depth. Story-telling through visual context." |
| `minimal` | "Single focal element with generous whitespace (60%+). Only essential shapes. Clean, reductive composition emphasizing a single visual idea." |

### Palette → Colors

| Palette | Description Fragment |
|---------|---------------------|
| `warm` | "Warm color scheme: orange (#E87040), golden yellow (#F5B041), terracotta (#C0764A), cream (#FFF8F0) background." |
| `elegant` | "Sophisticated palette: soft coral (#E8A598), muted teal (#6B9B8A), dusty rose (#D4A5A5), ivory (#FAF5F0) background." |
| `cool` | "Technical cool palette: engineering blue (#2E5090), navy (#1A2744), cyan accent (#4ECDC4), light gray (#F0F4F8) background." |
| `dark` | "Dark premium palette: charcoal (#1A1A2E), deep purple (#16213E), electric accent (#E94560), near-black background." |
| `earth` | "Natural earth tones: forest green (#4A7C59), warm brown (#8B6914), sage (#A8BDA0), warm beige (#F5F0E0) background." |
| `vivid` | "Vivid saturated palette: electric blue (#2563EB), hot pink (#EC4899), lime (#84CC16), white (#FFFFFF) background." |
| `pastel` | "Soft pastel palette: baby blue (#B8D4E3), blush pink (#F2D1D1), mint (#C8E6C9), soft lavender (#E1D5F0), white background." |
| `mono` | "Monochrome palette: rich black (#1A1A1A), dark gray (#4A4A4A), medium gray (#8A8A8A), light gray (#D0D0D0), white (#FAFAFA)." |
| `retro` | "Retro vintage palette: burnt sienna (#CC5A36), mustard (#D4A843), avocado (#6B7F3B), cream (#F5E6D0) background." |
| `duotone` | "Bold duotone palette using only two dominant colors with high contrast. Dramatic, poster-like visual impact." |

### Rendering → Style

| Rendering | Description Fragment |
|-----------|---------------------|
| `flat-vector` | "Clean flat vector style with solid color fills, crisp outlines, geometric shapes, and no gradients or shadows. Modern, icon-like elements." |
| `hand-drawn` | "Hand-drawn sketch style with organic, slightly imperfect pencil or pen strokes. Warm, personal, doodle-like quality with visible texture." |
| `painterly` | "Painterly artistic style with soft brush strokes, subtle color blending, watercolor-like washes, and gentle textural variation." |
| `digital` | "Polished digital illustration with precise lines, subtle gradients, soft shadows, and a clean professional finish." |
| `pixel` | "Pixel art style with visible square pixels, limited color palette, retro 16-bit aesthetic, and crisp blocky shapes." |
| `chalk` | "Chalkboard style with chalky white and colored lines on dark background, rough textured strokes, educational and approachable feel." |
| `screen-print` | "Bold screen-print poster style with limited flat colors, high contrast, halftone textures, and strong silhouettes." |

### Mood → Adjustments

| Mood | Description Fragment |
|------|---------------------|
| `subtle` | "Low contrast, muted and desaturated colors, light visual weight, calm and understated aesthetic." |
| `balanced` | "Medium contrast, normal saturation, balanced visual weight, clean and approachable." |
| `bold` | "High contrast, vivid saturated colors, heavy visual weight, dynamic energy and strong presence." |

### Font → Typography

| Font | Description Fragment |
|------|---------------------|
| `clean` | "Clean geometric sans-serif typography with modern, minimal letterforms." |
| `handwritten` | "Warm hand-lettered typography with organic brush strokes and a friendly, personal feel." |
| `serif` | "Elegant serif typography with refined letterforms and classic editorial character." |
| `display` | "Bold decorative display typography with heavy, expressive headline treatment." |

### Text Level → Content

| Text Level | Description Fragment |
|------------|---------------------|
| `none` | "No text elements. Pure visual composition." |
| `title-only` | "Include the title '[exact_title]' as a prominent text element." |
| `title-subtitle` | "Include the title '[exact_title]' prominently and a subtitle '[subtitle]' below it." |
| `text-rich` | "Include the title '[exact_title]', subtitle '[subtitle]', and 2-4 keyword tags." |

## Illustration Prompt Template

```
An illustration for a [topic] article, positioned [context].

The illustration visualizes [concept_from_section].
Key elements: [2-3 specific visual elements from the text].

Style: [match_article_tone - e.g., clean flat vector for technical, warm hand-drawn for personal].
Color scheme: [appropriate palette based on article mood].
[aspect_ratio] format. No text overlay. Clean composition.
```

## General Image Prompt Template

```
[User's description, expanded with specifics].

Style: [inferred or specified style].
Composition: [layout guidance].
[aspect_ratio] format.
```

## Prompt Quality Checklist

Before passing to the IDE tool, verify the prompt includes:

- [ ] Concrete visual elements (not abstract concepts)
- [ ] Specific color values or palette description
- [ ] Clear composition/layout direction
- [ ] Rendering style characteristics
- [ ] Aspect ratio specification
- [ ] "No realistic human faces" constraint (for covers)
- [ ] Sufficient detail for reproducibility (300+ characters)
