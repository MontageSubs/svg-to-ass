# SVG to ASS Draw Converter

**SVG to ASS Draw Converter · Vector Graphics in One Click**

> Convert SVG vector graphics into ASS/SSA subtitle format drawing commands in your browser — all processing happens locally.

<div align="right">

**[中文](./README.md) | English**

</div><br/>

<div align="center">

| [Open Tool](https://subs.js.org/svg-to-ass/) | [Report an Issue](https://github.com/MontageSubs/svg-to-ass/issues) | [Join the Discussion](https://github.com/MontageSubs/svg-to-ass/discussions) |
| :---: | :---: | :---: |

</div><br/>

## Overview

**SVG to ASS Draw Converter** is an open-source, browser-based tool developed by [@NickCollect](https://github.com/NickCollect) for converting SVG vector graphics into ASS/SSA `\p` drawing command code.

For typesetters, importing vector graphics (logos, geometric patterns, fine icons, etc.) from Adobe Illustrator or Inkscape into ASS/SSA subtitles has historically been a tedious workflow. Traditional solutions relied on outdated plugins that are difficult to install and prone to precision loss and jagged edges. This tool provides a **modern, install-free alternative** that runs entirely in the browser, supporting high-precision conversion and real-time preview, making complex vector effects creation simple and intuitive. All processing happens locally — your input data never leaves your device.

## Core Features

- **Process Directly in Browser** — All conversions run locally, supporting offline use, protecting privacy, no additional software installation required.
- **Multi-Format Input Support** — Paste SVG code, individual path data, or upload `.svg` files. Intelligently parses `<path>`, `<circle>`, `<rect>`, and other shape elements.
- **High-Precision Coordinate Scaling** — Offers 8x (`\p4`) and 16x (`\p5`) high-precision modes, fundamentally solving the jagged-edge problem in ASS/SSA `\p1` mode caused by limited integer precision.
- **Full Color / Stroke / Alpha / Blur Recovery** (on by default) — Per-shape ASS overrides: `\1c` (fill), `\3c` + `\bord` (stroke + width, auto-scaled by `\fscx`), `\1a` / `\3a` (alphas, automatically inverted to ASS convention), `\blur` (from `<feGaussianBlur>`). Reads inheritance from parent `<g>`, inline `style="..."`, and gradient `url(#id)` references resolved to their first `<stop>`. Consecutive same-style shapes are merged so you don't see redundant tag switches.
- **"Flat (legacy single path)" Toggle** — One checkbox restores the pre-color merged-single-block behavior with no color/stroke/alpha tags — useful for downstream pipelines that expect one uncolored shape. Naming and semantics aligned 1:1 with the sibling ass-to-svg tool's flat toggle.
- **Set origin to (0,0)** — One checkbox shifts every coordinate so the bounding box top-left lands at exactly `(0, 0)`. Pair with `\an7\pos(x,y)` and the drawing's top-left lands precisely at `(x, y)` on screen — no mental offset math.
- **Real-Time Vector Preview** — Preview pane renders the SVG's actual fills / strokes / alphas / blur (matching what libass will draw from the ASS output), with a brightness slider for inspecting dark shapes. Flat mode keeps the original accent-stroke geometry-inspection look.
- **One-Click ASS Command Generation** — Automatically prepends `\fscx1000\fscy1000` plus the per-shape color / stroke / alpha overrides described above; you can still append `\pos`, `\fs`, and other inline tags. Ready to copy and paste directly into ASS/SSA subtitles.

## Features

### Smart Path Extraction and Data Cleaning
Paste or upload SVG, the tool automatically parses the graphic structure, intelligently identifies boundaries and paths, precisely extracts vector information.

### High-Precision Coordinate Scaling
Provides **8x** (`\p4`) and **16x** (`\p5`) high-precision modes. By pre-scaling coordinates, the tool fundamentally solves the jagged-edge problem in ASS/SSA `\p1` mode caused by limited integer precision, making converted shapes smoother and more refined.

### Color / Stroke / Alpha / Blur Recovery (on by default)
Per-shape ASS color, stroke, alpha, and blur tags:
- Reads `fill` / `stroke` / `stroke-width` / `fill-opacity` / `stroke-opacity` / `opacity` / `filter`
- Includes inheritance from ancestor `<g>` elements, `style="..."` inline declarations, and `url(#id)` gradient definitions resolved to their first `<stop>` color
- Emits matching `\1c` / `\3c` / `\bord` / `\1a` / `\3a` / `\blur` tags; `\bord` and `\blur` auto-scale by the `\fscx` factor in your Extra Tags (default `\fscx1000` ⇒ ×10)
- Consecutive same-style shapes are merged into one block to avoid redundant tag switches

### "Flat (legacy single path)" Toggle
Step 2 has a `flatMode` checkbox — when checked, falls back to the merged single `\p1` block with no color / stroke / alpha tags. Useful for downstream pipelines that expect one uncolored shape. Persists across sessions via `localStorage`. Naming and semantics aligned 1:1 with ass-to-svg's flat toggle: both tools default to color-on; both let you opt out the same way.

### Set origin to (0,0)
Step 2 has a "Set origin to (0,0)" checkbox. When enabled, every coordinate is shifted so the bounding box top-left lands at exactly `(0, 0)`. Pair with `\an7\pos(x,y)` and the drawing's top-left lands precisely at `(x, y)` on screen — no mental offset math.

### Real-Time Vector Preview
The preview pane renders the SVG's actual fills, strokes, alphas, and blur (matching what libass will draw from the ASS output), with a brightness slider for inspecting dark shapes. Flat mode keeps the original accent-stroke geometry-inspection look.

### Flexible Tag Customization
Automatically generates `\fscx1000\fscy1000` and the per-shape color / stroke / alpha overrides described above; you can still append `\pos`, `\fs`, and other inline tags on top. Copy the output and paste directly into ASS/SSA subtitle lines — no further adjustments needed.

### Multiple Input Format Support
- Complete SVG source code (with DOCTYPE and styles)
- Individual path elements or raw path commands
- Direct `.svg` file upload
- **Special case**: Plain path input (no `<svg>` wrapper, just `m/l/b` commands or a bare `d` string) always falls back to a single `\p1` black block — preserving v3.11 behavior regardless of the colors / flat toggles.

## Usage

1. Open [https://subs.js.org/svg-to-ass/](https://subs.js.org/svg-to-ass/)
2. Paste or upload SVG code into the left input panel (Step 1)
3. Choose precision and configure additional tags in Step 2, then click **Convert**
4. Review the preview on the right, then click **Copy** and paste into your ASS/SSA subtitle

## Supported Input Formats

### SVG Standard Code

Paste complete SVG source code supporting various shape elements (path, circle, rect, etc.):

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <path d="M 50 2 C 76.5 2 98 23.5 98 50 ..."/>
  <circle cx="50" cy="50" r="40"/>
</svg>
```

### SVG Path Data

Single path element or raw path commands:

```svg
<path d="M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z"/>
```

Or input path directly:

```
M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z
```

### Upload SVG File

Upload a `.svg` file directly. The tool automatically reads and processes it.

## Precision Explanation

| Mode | Scale | ASS Tag | Use Case |
|------|-------|---------|----------|
| **8x** | ×8 | `\p4` | **Recommended** — balances precision and output length |
| **16x** | ×16 | `\p5` | Very small shapes or cases requiring maximum precision |

ASS/SSA subtitle drawing coordinates are integers. At `\p1` mode, precision is limited to 1 pixel per unit, causing jagged edges on small shapes. High-precision mode pre-scales coordinates so the effective resolution is much finer, resulting in smoother curves.

## Tech Stack

| Technology | Description |
|-----------|-------------|
| **HTML5 & CSS3** | Page structure and styling |
| **Vanilla JavaScript (ES6+)** | Core logic, zero external dependencies |
| **Native Math Calculations** | Bézier curve and elliptical arc conversion |
| **Local Browser Processing** | 100% client-side, no backend services |

## Repository Structure

```
svg-to-ass/
├── app/                      # Application source code
│   ├── index.html            # Main application file
│   ├── sw.js                 # Service Worker (caching strategy)
│   ├── manifests/            # PWA configuration (10 languages)
│   ├── sitemap.xml           # Search engine sitemap
│   └── icons/                # Application icons
├── README.md                 # Documentation in Chinese
├── README.en.md              # Documentation in English (this file)
├── CHANGELOG.md              # Change log (Bilingual)
└── LICENSE                   # MIT License
```

## Companion Tool

The reverse counterpart of this tool is [ASS to SVG Converter](https://subs.js.org/ass-to-svg/). The two tools' feature sets are now symmetric — both default to colors-on, both expose the same `Flat (legacy single path)` opt-out — together they support a full SVG ↔ ASS round-trip workflow.

## Localization

This tool fully supports **Chinese and English**, and also supports Spanish, Portuguese, Russian, Japanese, Korean, Arabic, Turkish, and more.

If you find translation errors or would like to help improve other languages, please share your suggestions in [Issues](https://github.com/MontageSubs/svg-to-ass/issues) or [Discussions](https://github.com/MontageSubs/svg-to-ass/discussions). Community contributions are welcome!

## Contributing

Contributions of all kinds are welcome, including but not limited to:

- **Feature Development** — New features, bug fixes, performance optimization, compatibility improvements
- **Documentation** — Improve README, add usage guides, write tutorials and best practices
- **Internationalization** — Translation improvements, new language support, localization optimization
- **Feedback & Suggestions** — Bug reports, feature requests, user experience feedback
- **Promotion & Sharing** — Recommend to friends, share on social media, write usage reviews

Join the discussion in [Issues](https://github.com/MontageSubs/svg-to-ass/issues) and [Discussions](https://github.com/MontageSubs/svg-to-ass/discussions). We'd love to have you!

### Contributors

<details>
<summary><strong>Core Team</strong></summary>

- **NickCollect** ([@NickCollect](https://github.com/NickCollect)) — Lead developer, core algorithms, frontend interaction design
- **Meow P** ([@mtsubs](https://github.com/mtsubs)) — Frontend UI development

</details>

## License

This project is licensed under the [MIT License](./LICENSE).

---

<div align="center">

**MontageSubs (蒙太奇字幕组)**  
"Powered by Love ❤️ 用爱发电"

</div>
