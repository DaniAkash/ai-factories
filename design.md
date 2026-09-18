# Automate your software delivery with AI Factories - Style Reference
> constellation floating on black velvet

**Theme:** dark

The deck runs as a dark-stage environment where black voids meet a single vivid violet accent, punctuated by amber sparks. Typography is monolithic and weightless: one typeface at weight 400 carries every heading at outsized scales with aggressive negative tracking, so headlines feel sculptural rather than informational. The visual signature is a constellation of tiny multicolored triangular particles drifting against the void, used as an ambient field on most spreads and as a dense organic cluster on the four hero moments. Layout follows a spacious two-column rhythm: oversized left-aligned headlines paired with generous body copy, floating on pure black with no panels, borders, or cards.

## Scale

The source style reference is authored for a 1280px page. This deck renders on a 1920px Slidev canvas, so **every size token is the spec value multiplied by 1.5**. Ratios, tracking relationships, and the 6px base unit are preserved exactly; only the absolute scale moves. Where this document lists a value, it is the deck value.

## Tokens - Colors

| Name | Value | Token | Role |
|------|-------|-------|------|
| Void | `#000000` | `--color-void` | Page canvas, every slide background, negative space. Pure black is the dominant surface, not dark gray. |
| Bone White | `#ffffff` | `--color-bone-white` | Headlines, primary body text. The only typographic color carrying maximum hierarchy. |
| Ash Gray | `#9a9a9a` | `--color-ash-gray` | Chrome and footer text, dimmed code, secondary labels. Recedes without going invisible. |
| Silver Mist | `#bdbdbd` | `--color-silver-mist` | Tertiary body text, caption-level information. |
| Electric Iris | `#8052ff` | `--color-electric-iris` | The single saturated violet. Accent words, numbered markers, the one pill button, the 70 side of the ratio bar. |
| Saffron Spark | `#ffb829` | `--color-saffron-spark` | Kicker labels, emphasis text, the line that carries the point. Warm against violet creates the chromatic tension. |
| Deep Verdant | `#15846e` | `--color-deep-verdant` | Particle palette member and subtle accent wash. Never a UI surface. |

Code syntax colors reuse the palette rather than introducing a second system: `.hi` bone white for the line that matters, `.vio` violet for structure, `.amb` amber for the punchline, `.off` `#3a3a3a` for context that should recede.

## Tokens - Typography

### PPNeueMontreal - single typeface across the deck · `--font-display`
- **Substitute:** Inter (what the deck actually loads; PPNeueMontreal is used when installed locally)
- **Weights:** 200, 400, 600
- **Role:** Display sizes carry headlines at **weight 400**, the same weight as body text. Massive scale creates hierarchy, never weight. Weight 200 is reserved for body copy, which is the signature choice: most decks use 400 for body, this one strips weight so paragraphs feel airy. Weight 600 at caption size serves chrome, footers, and kicker labels, uppercase with positive tracking.
- **OpenType:** `"ss01" on`

### IBM Plex Mono - code and diagrams · `--font-mono`
An extension. The source spec covers a marketing site and defines no code component. Monospace is introduced only for code, terminal output, and ASCII diagrams, and it honours the no-container rule below.

### Type Scale

| Role | Class | Size | Line Height | Letter Spacing |
|------|-------|------|-------------|----------------|
| caption | `.caption` | 18px | 1.5 | - |
| chrome / footer / kicker | `.chrome` `.foot` `.kicker` | 18-21px | 1.2 | 0.35-0.525px, uppercase |
| body | `.body` | 27px | 1.5 | - (weight 200) |
| heading xs | `.t-xs` | 40px | 1.0 | - |
| subheading | `.t-sub` | 54px | 1.2 | - |
| heading sm | `.t-sm` | 63px | 1.2 | -2.52px |
| heading | `.t-md` | 72px | 1.1 | -2.52px |
| heading lg | `.t-lg` | 117px | 1.1 | -4.68px |
| display | `.t-display` | 170px | 1.1 | -6.78px |

**Naming note, load-bearing.** Type classes are prefixed `t-` rather than the more natural `h-lg` / `h-sm`. UnoCSS is enabled, and `h-lg` is a height utility: it silently resolved to `height: 32rem` and collapsed every headline's box. Any new class added here must not collide with a utility namespace. When a heading renders at an unexpected size, check for a utility collision before checking the cascade.

**Specificity note.** Type classes are scoped under `.slide-shell`. The Slidev theme styles headings by element (`.slidev-layout h2`), which outranks a bare single-class token and silently overrode every size.

## Tokens - Spacing & Shapes

**Base unit:** 9px (6px spec, scaled). **Density:** comfortable to spacious.

| Name | Value | Token |
|------|-------|-------|
| 6 | 9px | `--spacing-6` |
| 12 | 18px | `--spacing-12` |
| 18 | 27px | `--spacing-18` |
| 24 | 36px | `--spacing-24` |
| 30 | 45px | `--spacing-30` |
| 36 | 54px | `--spacing-36` |
| 60 | 90px | `--spacing-60` |
| 96 | 144px | `--spacing-96` |
| 120 | 180px | `--spacing-120` |

**Border radius:** 36px for any radius element, `9999px` for pills. **Frame inset:** 144px left and right on every slide. **Chrome and footer:** 54px from the top and bottom edges.

## Components

### Slide shell
Every slide is `.slide-shell` > (`ParticleField`) + `.chrome` + `.frame` + `.foot`. The shell is pure void with `overflow: hidden`. The frame is a vertically centered flex column with the standard inset. Nothing else establishes a background.

### Chrome and footer
Transparent, sitting directly on the void. Chrome carries a short topical label on the left and the slide counter on the right. The footer carries the deck title on the left and the current section on the right. Both are 18px weight 600 uppercase in ash gray. No border, no backdrop, no fill.

### Kicker
The small uppercase amber label above a headline. 21px weight 600, `0.525px` tracking, `--color-saffron-spark`, 36px below it. One per slide at most.

### Section headline block
Two-column asymmetric `.split` (1.05fr / 0.95fr). Oversized left-aligned headline in white with negative tracking, body copy at weight 200 on the right or below. No boxes, no borders, pure typographic composition on black.

### Particle field
`<ParticleField variant="cloud|ambient|lanes" :opacity="n" />`. Canvas 2D, thousands of outlined 1px triangles in the full chromatic spectrum, drifting slowly and wrapping at the edges so density stays constant across a long talk. `cloud` is a dense organic cluster reserved for hero moments. `ambient` is a sparse background drift. `lanes` groups particles into three horizontal bands, used once, for the three-lanes spread. Never introduce photography or screenshots into a slide carrying a cloud.

### Numbered list
`.numbered` with a violet numeral in a fixed 72px column, a 36px title, and a 21px ash-gray description. No bullets, no containers, no dividers.

### Ratio bar
`.ratio` with proportional flex children, pill radius, pure fills, no border. Used once, for the 30/70 spread.

### Code and diagram block
`.code` (`.sm` / `.lg` variants). An extension. Monospace text directly on the void with `white-space: pre`, colored by role rather than contained. **Hierarchy comes from color, never from a panel.** There is no terminal chrome, no window frame, no background fill, no border.

### Pill button
The single filled violet element in the whole deck. Reserved for a genuine call to action; currently unused on any content slide.

## Do's and Don'ts

### Do
- Keep every slide background pure `#000000`. The void is the design.
- Set every headline at weight 400. Hierarchy comes from scale and negative tracking, never from bold.
- Keep body copy at weight 200. The ultra-light body is the signature.
- Reserve `--color-electric-iris` for accent words, numerals, and structure in code.
- Use `--color-saffron-spark` for the kicker and for the single line on a slide that carries the point.
- Let the particle field be the only imagery.
- Keep one idea per slide. Two at the absolute most.

### Don't
- Do not introduce cards, panels, borders, shadows, or background fills. Elements float on black with whitespace alone.
- Do not use violet as a large background block. It is an accent and a button color, not a surface.
- Do not set body text at weight 400.
- Do not add gradients to any component.
- Do not put planning vocabulary on a slide. Act numbers, section beats, stage directions, and notes to the speaker belong in presenter notes, never in the deck.
- Do not write copy that refers to the talk itself. The slide carries the content; the speaker carries the narration.
- Do not add a class whose name collides with a UnoCSS utility namespace.

## Elevation

None. No shadows anywhere. All hierarchy is scale, color contrast, and whitespace on a flat black canvas. A shadow would break the floating-in-space quality the particle field establishes.

## Imagery

Entirely procedural and abstract. The particle constellation is the visual brand: outlined triangles, 1px stroke, sharp-edged, in saturated chromatic colors, never grayscale. No photography, no product screenshots, no 3D renders, no stock illustration.

## Layout

Full-bleed void, 144px frame inset, content centered vertically. Most slides are a two-column asymmetric split; hero slides are a single left-aligned block with a particle cloud behind. Section gaps are generous. Density is deliberately low: one or two elements per viewport, never information-dense. The only exception is the twelve-invariant reference slide, which is dense on purpose because the audience is meant to photograph it.

## Quick Start

Run the deck:

```sh
cd slides
bun install
bun run dev
```

Tokens live in `slides/styles/index.css`. Components live in `slides/components/`.
