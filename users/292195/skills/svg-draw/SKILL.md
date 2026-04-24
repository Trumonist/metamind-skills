---
name: svg-draw
description: Generate SVG vector images from text descriptions and send as file. Triggers when user asks to draw/create/generate SVG icons, illustrations, logos, diagrams, vector images. Trigger words: "нарисуй svg", "создай svg", "сгенерируй svg", "svg картинку", "svg иконку", "svg схему", "svg логотип", "svg рисунок", "draw svg", "create svg", "generate svg", "svg image", "svg icon", "svg diagram".
---

# SVG Draw Skill

Generates SVG vector images from user descriptions and delivers the `.svg` file via Telegram upload.

## When to use

- User asks to draw / create / generate SVG specifically
- Keywords: нарисуй svg, создай svg, сгенерируй svg, svg картинку, svg иконку, svg схему, svg логотип, draw svg, create svg, generate svg, svg image, svg icon

## When NOT to use

- User wants a raster image (jpg/png) — use image-gen skill instead
- User wants a Mermaid diagram — use mermaid

## Step-by-step instructions

### 1. Parse the request

Extract:
- **Subject**: what to draw (icon, logo, scene, diagram, flag, chart, etc.)
- **Style**: minimal / flat / outline / detailed / isometric / cartoon (default: flat)
- **Colors**: explicit palette or derive sensibly from subject
- **Size**: default `viewBox="0 0 400 400"` unless user specifies

For simple requests: make a creative decision and proceed immediately.  
For complex scenes (>5 elements): ask **one** clarifying question max.

### 2. Generate SVG code

Write clean, valid SVG:
- Open tag: `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 W H" width="W" height="H">`
- Group related elements with `<g id="...">` 
- Use `<defs>` for gradients/filters
- Add `<!-- comments -->` marking major sections
- Ensure all tags are closed, all attributes quoted
- No external http image references
- Use `font-family="Arial, sans-serif"` for text elements
- Minimum viewBox: 200×200

### 3. Save and send file

```bash
cat > /tmp/image.svg << 'SVGEOF'
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" width="400" height="400">
  <!-- generated content -->
</svg>
SVGEOF

curl -sS -F file=@/tmp/image.svg \
     -F token=$UPLOAD_TOKEN \
     $UPLOAD_URL
```

Use a descriptive filename matching the subject (e.g. `house-icon.svg`, `solar-system.svg`).

### 4. Show code preview

After sending, show the first ~25 lines of the SVG in a ` ```xml ` block so user can preview the structure.

End every response with:
> 📎 SVG файл отправлен. Открой в браузере или редакторе (Inkscape, Figma, VS Code).

## Quality checklist

- [ ] Valid XML — all tags closed
- [ ] No external dependencies
- [ ] viewBox ≥ 200×200
- [ ] Visually meaningful content (not just a rectangle)
- [ ] File sent via curl before showing preview

## Examples

| Request | Output |
|---|---|
| нарисуй svg иконку домика | Flat house icon 48×48 |
| создай svg логотип буква A в синем круге | Circle + centered A, gradient |
| svg схема солнечной системы | Dark bg, concentric orbits, labeled planets |
| svg флаг России | White/blue/red stripes, correct 2:3 ratio |
