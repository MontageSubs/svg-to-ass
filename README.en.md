# SVG to ASS Draw Converter
**SVG 转 ASS 绘图指令 · MontageSubs**

<br/>

> **Convert SVG vector paths into Aegisub ASS drawing commands, entirely in your browser — no installation required.**
>
> *All processing runs locally. No files are ever uploaded to a server.*

<div align="right">

**[中文](./README.md) | English**

</div><br/>

<div align="center">

| [Open Tool](https://subs.js.org/svg-to-ass/) | [Report an Issue](https://github.com/MontageSubs/svg-to-ass/issues) | [Join the Discussion](https://github.com/MontageSubs/svg-to-ass/discussions) |
| :---: | :---: | :---: |

</div><br/>

## Overview

The SVG to ASS Draw Converter is an open-source, browser-based utility developed by @NickCollect for converting SVG vector paths into Aegisub ASS `\p` drawing command code.

For typesetters, importing vector graphics (logos, geometric patterns, etc.) from Adobe Illustrator or Inkscape into Aegisub has historically been a tedious workflow. Traditional solutions relied on outdated plugins that are difficult to install and prone to precision loss and jagged edges. This tool provides a modern, install-free alternative that runs entirely in the browser.

Everything processes locally. Your inputs stay on your device.

## Features

**Smart Path Extraction and Dirty-Data Cleaning**

Paste your full SVG source code directly. The tool automatically locates all `<path>` elements, extracts their `d` attribute data, merges multiple paths, and converts them in one step. No manual tag cleanup required. If the code also contains unconverted basic shape elements, the tool extracts whatever paths it can find and displays a warning.

**High-Precision Coordinate Scaling**

Offers two high-precision modes: 8x (`\p4`) and 16x (`\p5`). By pre-scaling coordinates before conversion, the tool eliminates the jagged edges that occur with small shapes in Aegisub's `\p1` mode, where integer precision is limited to one pixel.

**Real-Time Vector Preview**

Renders the converted shape instantly in the right panel. A brightness slider lets you adjust the preview background to suit both dark and light-colored graphics.

**Automatic ASS Header Generation**

Automatically prepends `\fscx1000\fscy1000` and the appropriate `\p` tag. Supports custom inline tags such as `\pos` or `\c`. The output is ready to paste directly into an Aegisub dialogue line.

**Bilingual Interface (Chinese / English)**

Automatically follows the browser language (Chinese environments default to Chinese, all others default to English). A toggle button in the top-right corner lets you switch at any time, and your preference is saved across sessions.

## Usage

No installation needed:

1. Open [https://subs.js.org/svg-to-ass/](https://subs.js.org/svg-to-ass/)
2. Paste SVG code into the left input panel (Step 1)
3. Choose precision and configure extra tags in Step 2, then click **Convert**
4. Check the preview on the right, then click **Copy** and paste the result into Aegisub

---

## Supported Input Formats

**SVG Standard Code**

Paste complete SVG source code. Supports various shape elements (path, circle, rect, etc.):

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <path d="M 50 2 C 76.5 2 98 23.5 98 50 ..."/>
  <circle cx="50" cy="50" r="40"/>
</svg>
```

**SVG Path Data**

Single path element or raw path commands:

```svg
<path d="M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z"/>
```

```
M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z
```

**Upload SVG File**

Upload a `.svg` file directly. The tool automatically reads and processes it.


---

## Precision Modes

| Mode | Scale | ASS Tag | Use Case |
|------|-------|---------|----------|
| 8x | ×8 | `\p4` | Recommended — balances precision and output length |
| 16x | ×16 | `\p5` | Very small shapes or cases requiring maximum precision |

Aegisub drawing coordinates are integers. At `\p1`, precision is limited to 1 pixel per unit, which causes jagged edges on small shapes. High-precision mode pre-scales coordinates so the effective resolution is much finer, resulting in smoother curves.

---

## Tech Stack

- Pure HTML5 & CSS3
- Vanilla JavaScript (ES6+)
- Native math (Bézier curve and elliptical arc conversion)
- No backend — 100% local browser processing

## Repository Structure

```
svg-to-ass/
├── index.html        # Tool (single-file, all logic included)
├── manifest.json     # PWA manifest (Chinese)
├── manifest-en.json  # PWA manifest (English)
├── sw.js             # Service Worker (caching strategy)
├── LICENSE
├── README.md         # Chinese documentation (primary)
└── README.en.md      # English documentation (this file)
```

## Contributing

Contributions of all kinds are welcome:

- Submit bug reports or feature requests via [Issues](https://github.com/MontageSubs/svg-to-ass/issues)
- Share usage tips or engage in technical discussion via [Discussions](https://github.com/MontageSubs/svg-to-ass/discussions)
- Submit a Pull Request to improve code or documentation

## Changelog

### v3.3
- Support pasting full SVG code and uploading SVG files
- Support parsing multiple shape elements like circle and rect

### v3.2
- UI interface improvements
- Added multi-language support

### v3.1
- Added bilingual interface (Chinese / English): automatically follows the browser language, with a manual toggle button in the top-right corner; preference is saved to localStorage across sessions
- Full i18n coverage: all UI text, placeholders, toast messages, error strings, and the Giscus discussion language update on switch
- Implemented as a lightweight i18n engine using `data-i18n` attributes and a `t()` helper function — no external dependencies, single-file architecture unchanged

### v3.0.1
- Fixed silent zero-fill on missing path arguments: truncated path data (e.g. a `C` command with two arguments instead of six) previously filled missing values with `0`, producing incorrect output without any warning. The parser now throws a descriptive error immediately, e.g. `路径数据不完整：命令 "C" 缺少参数`
- Fixed multi-line `d` attribute extraction: paths exported by Illustrator, Inkscape, or AI tools often span multiple lines inside the `d` attribute. The previous regex `[^"']+` did not match newline characters, silently dropping entire paths. Changed to `[\s\S]*?` to handle multi-line values correctly
- Fixed `M` command incorrectly influencing preview bounding box: `moveto` does not render geometry and should not affect `viewBox` computation. When a path began with a distant `M`, the preview shape appeared unnecessarily scaled down
- Fixed preview stroke becoming invisible on large-coordinate SVGs: replaced fixed `stroke-width="1"` with `vector-effect: non-scaling-stroke` so the stroke remains visible at any coordinate scale
- Fixed arc segments silently disappearing at near-zero angles: added `Math.max(1, ...)` guard to `arcToBezier` segment count to prevent floating-point precision from producing zero iterations
- Fixed background brightness slider missing accessible label association: added `for="bgSlider"` to the `<label>` element so screen readers correctly identify the control
- Removed unreachable dead code from precision tag map: `\p1` and `\p3` entries were never reachable from the UI and have been removed

### v3.0
- Fixed inaccurate preview bounding box: viewBox was previously computed from Bézier control points rather than actual curve extrema, causing shapes to appear incorrectly scaled or offset in the preview. Now uses derivative roots for precise bounds
- Fixed silent NaN propagation in `S`/`T` commands caused by uninitialized control point variables
- Fixed copy button API priority: now correctly prefers `navigator.clipboard`, with deprecated `document.execCommand` as a legacy fallback
- Fixed icon MIME type declaration (`image/jpeg` → `image/png`)
- Refactored path parser `switch` block: replaced all `var` with `let`/`const` inside explicit block scopes, eliminating cross-case variable hoisting issues
- Moved toast timer ID from a DOM property to a closure variable
- Added input size guard (>500 KB) to warn users before synchronous processing may block the main thread

### v2.9
- Initial public release

---

## License

This project is licensed under the [MIT License](./LICENSE).

---

<div align="center">

**MontageSubs (蒙太奇字幕组)**  
"Powered by Love ❤️ 用爱发电"

</div>
