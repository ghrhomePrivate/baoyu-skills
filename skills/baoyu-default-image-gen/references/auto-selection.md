# Auto-Selection Rules

When a dimension is omitted, select based on content signals.

## Auto Type Selection

| Signals | Type |
|---------|------|
| Product, launch, announcement, release, reveal | `hero` |
| Architecture, framework, system, API, technical, model | `conceptual` |
| Quote, opinion, insight, thought, headline, statement | `typography` |
| Philosophy, growth, abstract, meaning, reflection | `metaphor` |
| Story, journey, travel, lifestyle, experience, narrative | `scene` |
| Zen, focus, essential, core, simple, pure | `minimal` |

## Auto Palette Selection

| Signals | Palette |
|---------|---------|
| Personal story, emotion, lifestyle, human | `warm` |
| Business, professional, thought leadership, luxury | `elegant` |
| Architecture, system, API, technical, code | `cool` |
| Entertainment, premium, cinematic, dark mode | `dark` |
| Nature, wellness, eco, organic, travel | `earth` |
| Product launch, gaming, promotion, event | `vivid` |
| Fantasy, children, gentle, creative, whimsical | `pastel` |
| Zen, focus, essential, pure, simple | `mono` |
| History, vintage, retro, classic, exploration | `retro` |
| Movie poster, album cover, concert, cinematic, dramatic | `duotone` |

## Auto Rendering Selection

| Signals | Rendering |
|---------|-----------|
| Clean, modern, tech, icon-based, infographic | `flat-vector` |
| Sketch, note, personal, casual, doodle, warm | `hand-drawn` |
| Art, watercolor, soft, dreamy, creative, fantasy | `painterly` |
| Data, dashboard, SaaS, corporate, polished | `digital` |
| Gaming, retro, 8-bit, nostalgic | `pixel` |
| Education, tutorial, classroom, teaching | `chalk` |
| Poster, movie, album, concert, silhouette | `screen-print` |

## Auto Text Selection

| Signals | Text Level |
|---------|------------|
| Visual-only, photography, abstract, art | `none` |
| Article, blog, standard cover | `title-only` |
| Series, tutorial, technical with context | `title-subtitle` |
| Announcement, features, multiple points | `text-rich` |

Default: `title-only`

## Auto Mood Selection

| Signals | Mood Level |
|---------|------------|
| Professional, corporate, academic, luxury | `subtle` |
| General, educational, standard, blog | `balanced` |
| Launch, announcement, promotion, gaming | `bold` |

Default: `balanced`

## Auto Font Selection

| Signals | Font |
|---------|------|
| Personal, lifestyle, human, warm, friendly | `handwritten` |
| Technical, professional, clean, modern, minimal | `clean` |
| Editorial, academic, luxury, classic, literary | `serif` |
| Announcement, entertainment, promotion, bold | `display` |

Default: `clean`

## Palette ↔ Rendering Compatibility

Best combinations (✓✓ = excellent, ✓ = good):

| Palette \ Rendering | flat-vector | hand-drawn | painterly | digital | pixel | chalk | screen-print |
|---------------------|:-----------:|:----------:|:---------:|:-------:|:-----:|:-----:|:------------:|
| warm | ✓✓ | ✓✓ | ✓ | ✓ | | ✓ | |
| elegant | ✓✓ | ✓ | ✓✓ | ✓✓ | | | ✓ |
| cool | ✓✓ | | | ✓✓ | | | |
| dark | ✓ | | ✓ | ✓✓ | | | ✓✓ |
| earth | ✓ | ✓✓ | ✓✓ | | | ✓ | |
| vivid | ✓✓ | | | ✓ | ✓✓ | | ✓ |
| pastel | ✓✓ | ✓✓ | ✓ | | | ✓✓ | |
| mono | ✓✓ | ✓✓ | | ✓ | | ✓ | ✓ |
| retro | | ✓ | | | ✓✓ | | ✓✓ |
| duotone | ✓ | | | ✓ | | | ✓✓ |
