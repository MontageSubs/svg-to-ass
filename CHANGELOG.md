# Changelog | 更新日志

## v3.14

<details>
<summary><strong>中文版</strong></summary>

- **补齐 SVG class 样式颜色解析**：转换器现在会读取 `<style>` 中的简单 class 规则，例如 `.cls-1 { fill: #ff0000; }` 和 `path.cls-1 { stroke: #0000ff; }`，适配 Illustrator / Image Trace 常见导出格式。
- **保留现有样式优先级**：行内 `style="..."` 优先于 `<style>` class 规则；class 规则优先于 `fill=""` / `stroke=""` 等展示属性；父级 `<g class="...">` 可继续继承到子路径。
- **转换边界不变**：仍聚焦矢量 path / 基础形状到 ASS 绘图指令，不做位图、复杂选择器或完整 CSS 引擎。

</details>

<details>
<summary><strong>English</strong></summary>

- **Added SVG class style color parsing**: The converter now reads simple class rules inside `<style>`, such as `.cls-1 { fill: #ff0000; }` and `path.cls-1 { stroke: #0000ff; }`, matching common Illustrator / Image Trace exports.
- **Preserved style precedence**: Inline `style="..."` wins over `<style>` class rules; class rules win over presentation attributes like `fill=""` / `stroke=""`; parent `<g class="...">` styles still inherit into child paths.
- **Conversion scope unchanged**: The tool remains focused on vector paths / basic shapes to ASS drawing commands, without raster image tracing, complex selectors, or a full CSS engine.

</details>

## v3.13

<details>
<summary><strong>中文版</strong></summary>

- **「原点对齐」从勾选项升级为三档下拉**：Step 2 的「原点对齐到 (0,0)」勾选项替换为 `Origin` 下拉，三档可选：
  - **保持原坐标**（默认）—— 与 v3.11 / v3.12 不勾选时的行为一致，输出按 SVG `viewBox` 原点
  - **左上角对齐到 (0,0)** —— 等同于旧版勾上的行为，配合 `\an7\pos(x,y)` 让图形左上角精确落在 `(x, y)`
  - **中心对齐到 (0,0)**（新增）—— 把整张图的包围盒中心移到 `(0, 0)`，配合 `\an5\pos(x,y)` 让中心精确落在 `(x, y)`，省去手动算 `width/2, height/2` 偏移
- **localStorage 自动迁移**：老用户的 `svg2ass-rebase = '1'` 自动迁移为 `svg2ass-origin = 'topleft'`，老 key 同步清理；未勾选过的用户继续默认 `none`，零回归
- **重命名说明**：旧名「Set origin to (0,0)」实际含义是「左上角到 (0,0)」，新下拉直接用 `Top-left at (0,0)` / `Center at (0,0)` 表述，去除歧义
- **i18n**：10 种语言全部新增 `label-origin` + 三个 option 字符串；老的 `label-rebase` 删除
- 源自 [Issue #3](https://github.com/MontageSubs/svg-to-ass/issues/3)

</details>

<details>
<summary><strong>English</strong></summary>

- **"Origin" upgraded from a checkbox to a 3-way dropdown**: The "Set origin to (0,0)" checkbox in Step 2 is replaced with an `Origin` `<select>` with three options:
  - **None** (default) — same as v3.11 / v3.12 with the box unchecked: output uses the SVG's `viewBox` origin
  - **Top-left at (0,0)** — same as the old checkbox checked: pair with `\an7\pos(x,y)` to land the drawing's top-left at `(x, y)` on screen
  - **Center at (0,0)** (new) — shifts the bounding-box center to `(0, 0)`; pair with `\an5\pos(x,y)` to land the drawing's center at `(x, y)` without doing `width/2, height/2` math
- **localStorage auto-migration**: Existing users with `svg2ass-rebase = '1'` are migrated to `svg2ass-origin = 'topleft'`, and the old key is cleaned up. Users who never checked the box continue to get `none`, zero regression
- **Naming clean-up**: The old "Set origin to (0,0)" actually meant "top-left to (0,0)", which was ambiguous. The new dropdown uses `Top-left at (0,0)` / `Center at (0,0)` so the semantics are explicit
- **i18n**: `label-origin` + three option strings added to all 10 languages; the old `label-rebase` key was removed
- From [Issue #3](https://github.com/MontageSubs/svg-to-ass/issues/3)

</details>

## v3.12

<details>
<summary><strong>中文版</strong></summary>

- **新增：颜色还原，默认开启（与 ass-to-svg 行为对称）**：转换流程从「合并所有路径为单一 `\p1` 块、忽略所有颜色信息」升级为按形状逐组发出 ASS 颜色与描边/透明度/模糊标签：
  - 解析 SVG 的 `fill` / `stroke` / `stroke-width` / `fill-opacity` / `stroke-opacity` / `opacity` / `filter`（含 `<g>` 上的继承属性、`style="..."` 内联样式、`url(#id)` 渐变定义的首个 `<stop>` 颜色）
  - 输出对应的 `\1c` / `\3c` / `\bord` / `\1a` / `\3a` / `\blur` 标签；同色形状自动合并为同一段，不会冗余切换
  - 预览区同步显示 SVG 原始颜色与描边/透明/模糊效果（之前永远是占位单色 `#3d4150`）
- **新增「扁平模式（单 path）」勾选项 — 默认关闭**：Step 2 增加 `flatMode` 勾选，勾上回退到 v3.11 的合并单 `\p1` 块行为，关闭所有颜色/描边/透明度标签输出，便于下游流程作为单一无色形状处理；勾选状态写入 `localStorage` 跨会话保留。命名与 ass-to-svg v1.4 的 `Flat (legacy single path)` 完全对齐——两个工具的勾选项语义完全对称：默认都开颜色、勾上都关
- **行为兼容性**：当输入是无 SVG 标签的纯路径数据（仅 `m/l/b` 命令或 `d` 字符串），无论是否勾选 flat 模式，输出都退化为单 `\p1` 黑色块，保持 v3.11 行为不变

</details>

<details>
<summary><strong>English</strong></summary>

- **New: color recovery, on by default (symmetric with ass-to-svg)**: Conversion upgrades from "merge all paths into one `\p1` block, drop all color information" to per-shape grouping with ASS color, stroke, alpha, and blur tags:
  - Parses `fill` / `stroke` / `stroke-width` / `fill-opacity` / `stroke-opacity` / `opacity` / `filter` from each shape (with inheritance from ancestor `<g>` elements, `style="..."` inline declarations, and `url(#id)` gradient definitions resolved to their first `<stop>` color)
  - Emits matching `\1c` / `\3c` / `\bord` / `\1a` / `\3a` / `\blur` tags; consecutive same-style shapes are merged into one block to avoid redundant tag switches
  - Preview now mirrors the SVG's actual fills, strokes, opacities, and blur (previously always rendered the placeholder color `#3d4150`)
- **New "Flat (legacy single path)" checkbox — off by default**: Step 2 adds a `flatMode` toggle that, when checked, falls back to the v3.11 merged single `\p1` block with no color/stroke/alpha tags — useful for downstream pipelines that expect one uncolored shape. Persists across sessions via `localStorage`. The naming and semantics are aligned 1:1 with ass-to-svg v1.4's `Flat (legacy single path)` toggle: both tools default to color-on; both let you opt out the same way
- **Compatibility note**: For plain path input (no SVG markup, just `m/l/b` commands or a bare `d` string) the output reduces to a single black `\p1` block regardless of the checkbox — preserving v3.11 behavior for that input shape

</details>

## v3.11

<details>
<summary><strong>中文版</strong></summary>

- **主题切换逻辑重构 — 默认始终跟随系统**：之前点过日/月按钮的偏好会写入 `localStorage` 跨会话保留，现在改为：页面主题永远跟随用户系统设置（`prefers-color-scheme`），且**实时跟随**——使用页面期间在 macOS 系统设置切换 light/dark，页面立即响应（之前需要刷新）。日/月按钮仍可用，但仅作为**当前 session 的临时覆盖**，刷新后回到跟随系统。理由：用户系统是 dark mode 通常意味着当下就想看 dark；网页强行记住一次手动选择反而违背用户当前意图。老用户残留的 `localStorage.theme` 会在下次访问时自动清除
- **评论区主题与页面同步**：giscus 评论框现在跟随页面当前主题（之前总是直接读系统 `prefers-color-scheme`，用户手动覆盖主题后评论框颜色不一致）；浏览过程中切换主题，已加载的评论框也会通过 `postMessage` 实时跟随
- **新增：用户参数持久化**：以下设置现在保存到 `localStorage`，刷新页面后保持不变（之前每次都回默认）：
  - 「Background」滑块位置
  - 「Precision (Scale)」精度选择
  - 「Extra Tags」额外标签输入框
  
  「Rebase to (0,0)」勾选状态原本就是持久化的，未变化

</details>

<details>
<summary><strong>English</strong></summary>

- **Theme logic refactor — always follows system by default**: Previously, clicking the sun/moon button persisted the choice to `localStorage` across sessions. Now: the page theme always follows the user's system setting (`prefers-color-scheme`), and **tracks live** — switching macOS system theme while using the page applies immediately (previously required a reload). The sun/moon button still works but only as a **session-only override**; reload restores following the system. Rationale: a user with system dark mode usually wants to see dark **right now**; forcibly remembering a one-time manual choice across sessions undermines current intent. Stale `localStorage.theme` entries from older versions are automatically cleared on next visit
- **Comments theme synced with page**: The giscus comment widget now matches the page's current theme (previously read `prefers-color-scheme` directly, mismatching whenever the user manually overrode the page theme); already-loaded comment widgets also follow via `postMessage` when the user toggles theme mid-session
- **New: user parameters persisted**: The following settings are now saved to `localStorage` and survive page reloads (previously reset to defaults every time):
  - "Background" slider position
  - "Precision (Scale)" selector
  - "Extra Tags" text input
  
  The "Rebase to (0,0)" checkbox was already persisted; unchanged

</details>

## v3.10

<details>
<summary><strong>中文版</strong></summary>

- **修复 dark mode 下两个 slider 相关视觉问题**：
  - 「背景亮度」滑块轨道在 dark mode 下完全不可见（轨道颜色写死为深色低透明，在深色背景上隐形）；现在 dark mode 加了独立的浅色轨道样式
  - 预览区域在页面初次加载时显示亮白色覆盖层（`#previewBg` CSS 默认值是 `rgba(255,255,255,0.92)`，但 slider 的 JS 监听器只在 `input` 事件时才更新背景，导致默认状态错位）；现在 CSS 默认改为完全透明，并在页面加载时主动调用一次 slider handler，让 `value="49.6"` 的状态从首屏就正确应用

</details>

<details>
<summary><strong>English</strong></summary>

- **Fixed two dark-mode slider issues**:
  - The "Background brightness" slider track was invisible in dark mode (the track color was hard-coded to a low-opacity dark slate that blended into dark surfaces); added an independent light-tinted track style for `html[data-theme="dark"]`
  - On first paint, the preview area showed a bright white overlay (the `#previewBg` CSS default was `rgba(255,255,255,0.92)`, and the slider's `input` listener only fires on user interaction, so the default state was inconsistent with the slider's `value="49.6"`); CSS default now starts transparent, and the slider handler is invoked once on page load so the initial state matches the slider value

</details>

## v3.9

<details>
<summary><strong>中文版</strong></summary>

- 预览背景默认透明度调整：`bgSlider` 默认值 90 → 49.6（几乎透明），让 Liquid Glass 多色玻璃质感透出来；slider step 改为 0.1 以保留精确控制

</details>

<details>
<summary><strong>English</strong></summary>

- Preview background opacity default tuned: `bgSlider` default value 90 → 49.6 (near-transparent), letting the Liquid Glass color tones show through; slider step changed to 0.1 to keep precise control

</details>

## v3.8

<details>
<summary><strong>中文版</strong></summary>

- **设计大改**：全面切换到 Liquid Glass 视觉语言——多色径向渐变背景（黄/天空蓝/粉/靛/绿），玻璃卡片（`backdrop-filter: blur(24px) saturate(180%)`），pill 按钮（`border-radius: 999px`），三色 step 头（蓝/黄/天空），渐变色 step number 徽标，圆角 18px 卡片
- **字体升级**：Inter（UI）+ JetBrains Mono（代码），从系统字栈切换为 web fonts
- **新增 Light/Dark 主题切换按钮**：header 右侧加月亮/太阳图标按钮，手动切换不依赖系统偏好；偏好写入 `localStorage`，跨会话保留；首次访问自动跟随 `prefers-color-scheme`
- **品牌标识强化**：header 加 "S/A" 渐变圆形 brand-mark 徽章，提升识别度
- 保留所有 v3.6/v3.7 功能：原点对齐到 (0,0)、ASS 转 SVG 跨工具入口、5 档精度、10 种语言、PWA 离线

</details>

<details>
<summary><strong>English</strong></summary>

- **Design overhaul**: full visual rework to the Liquid Glass design language — multi-color radial-gradient background (yellow / sky / pink / indigo / green), glassmorphism cards (`backdrop-filter: blur(24px) saturate(180%)`), pill buttons (`border-radius: 999px`), tri-color step heads (blue / yellow / sky), gradient step-number badges, 18px card radius
- **Typography upgrade**: Inter (UI) + JetBrains Mono (code), replacing the system font stack
- **Light/Dark theme toggle**: new sun/moon button in the header for manual switching independent of system preference; choice persists in `localStorage` across sessions; first-time visit follows `prefers-color-scheme`
- **Brand-mark badge**: gradient "S/A" circular mark added to the header for identity reinforcement
- All v3.6/v3.7 features preserved: origin-to-(0,0) checkbox, ASS-to-SVG sibling-tool link, five precision tiers, 10 locales, PWA offline support

</details>

## v3.7

<details>
<summary><strong>中文版</strong></summary>

- 新增配套工具入口：在 header 添加 [ASS 转 SVG](https://subs.js.org/ass-to-svg/) 链接，与现有 ASS Subsetter 平级。配合新工具可完成 SVG ↔ ASS 双向转换工作流

</details>

<details>
<summary><strong>English</strong></summary>

- Added companion tool link in the header: [ASS to SVG](https://subs.js.org/ass-to-svg/), as a sibling to the existing ASS Subsetter. Together they enable a full SVG ↔ ASS round-trip workflow

</details>

## v3.6

<details>
<summary><strong>中文版</strong></summary>

- 新增「原点对齐到 (0,0)」选项：转换前可勾选此项，工具会自动平移所有坐标，让图形包围盒的左上角对齐到 ASS 坐标 (0,0)，配合 `\an7` + `\pos(x,y)` 使用时所见即所得，无需手动算偏移
- 偏好写入 localStorage 跨会话保留；默认关闭，不影响现有用户习惯
- 包围盒计算包含所有 `M` 端点与贝塞尔极值，移位精度等于实际渲染几何

</details>

<details>
<summary><strong>English</strong></summary>

- Added "Set origin to (0,0)" option: when enabled, all coordinates are shifted so the bounding box top-left lands at ASS (0,0), making `\an7` + `\pos(x,y)` placement WYSIWYG with no manual offset math
- Preference is persisted to localStorage across sessions; off by default to preserve existing behavior
- Bounding box uses all `M` endpoints and Bézier curve extrema, so the shift matches the actual rendered geometry exactly

</details>

## v3.5

<details>
<summary><strong>中文版</strong></summary>

- 支持 p1 至 p5 五档精度等级，用户可根据需求灵活选择
- p1 和 p3 精度下保留小数点，进一步提升转换精度
- 预览结果反推优化：基于最终生成结果进行反向计算，确保预览与实际输出一致
- 新增 SVG Transform 属性支持，更好兼容复杂 SVG 变换
- ViewBox 坐标系识别：自动识别并计算 SVG viewBox 属性，精确定位坐标系原点

</details>

<details>
<summary><strong>English</strong></summary>

- Support for five precision levels (p1–p5) enabling flexible selection based on user requirements
- Decimal preservation at p1 and p3 precision levels for enhanced conversion accuracy
- Improved preview validation through reverse-calculation against the final output to ensure consistency
- Added SVG Transform attribute support with improved compatibility for complex transformations
- ViewBox coordinate system recognition featuring automatic detection and calculation of viewBox attributes for precise coordinate origin positioning

</details>

## v3.4

<details>
<summary><strong>中文版</strong></summary>

- 完善多语言支持（10 种语言）和 PWA 离线
- 优化元数据

</details>

<details>
<summary><strong>English</strong></summary>

- Enhanced multi-language support (10 languages) and improved PWA offline functionality
- Optimized metadata

</details>

## v3.3

<details>
<summary><strong>中文版</strong></summary>

- 支持完整 SVG 代码粘贴和文件上传
- 支持 circle、rect 等多种图形元素解析

</details>

<details>
<summary><strong>English</strong></summary>

- Support pasting full SVG code and uploading SVG files
- Support parsing multiple shape elements like circle and rect

</details>

## v3.2

<details>
<summary><strong>中文版</strong></summary>

- UI 界面优化
- 新增多语言支持

</details>

<details>
<summary><strong>English</strong></summary>

- UI interface improvements
- Added multi-language support

</details>

## v3.1

<details>
<summary><strong>中文版</strong></summary>

- 新增中英双语界面：自动跟随浏览器语言（中文环境默认中文，其余默认英文），右上角可随时手动切换，偏好写入 localStorage 跨会话保存
- 双语覆盖全量：UI 文字、placeholder、toast 提示、错误信息、Giscus 讨论区语言均随切换同步更新
- 采用轻量 i18n 引擎（`data-i18n` 属性 + `t()` 函数），零外部依赖，单文件架构不变

</details>

<details>
<summary><strong>English</strong></summary>

- Added bilingual interface (Chinese / English): automatically follows the browser language, with a manual toggle button in the top-right corner; preference is saved to localStorage across sessions
- Full i18n coverage: all UI text, placeholders, toast messages, error strings, and the Giscus discussion language update on switch
- Implemented as a lightweight i18n engine using `data-i18n` attributes and a `t()` helper function — no external dependencies, single-file architecture unchanged

</details>

## v3.0.1

<details>
<summary><strong>中文版</strong></summary>

- **修复路径参数缺失时静默补零的问题**：残缺路径数据（如 `C` 命令只有两个参数而非六个）原本会用 `0` 填充剩余坐标，生成错误输出却无任何提示；现改为立即抛出明确错误，如「路径数据不完整：命令 "C" 缺少参数」

- **修复多行 `d` 属性被漏提取的问题**：Illustrator、Inkscape 及部分 AI 工具导出的 SVG 路径值常跨行写入，原正则 `[^"']+` 不匹配换行符导致整条路径丢失；改用 `[\s\S]*?` 后正确支持多行属性值

- **修复 `M` 命令错误纳入预览包围盒**：`moveto` 不渲染几何体，不应影响 viewBox 计算；原实现在路径以远处 `M` 开头时会导致预览图形被不必要地缩小

- **修复大坐标 SVG 预览描边消失**：将固定 `stroke-width="1"` 改为 `vector-effect: non-scaling-stroke`，在任意坐标尺度下描边均清晰可见

- **修复弧线在极小角度时静默消失**：`arcToBezier` 中 `segments` 加入 `Math.max(1, ...)` 保护，防止浮点精度导致分段数为零

- **修复背景亮度滑块无障碍标签未关联**：`<label>` 补充 `for="bgSlider"`，屏幕阅读器现可正确识别控件

- **移除精度映射表中无法命中的死代码**：`\p1` 与 `\p3` 条目在 UI 中不可选，已清除

</details>

<details>
<summary><strong>English</strong></summary>

- **Fixed silent zero-fill on missing path arguments**: truncated path data (e.g. a `C` command with two arguments instead of six) previously filled missing values with `0`, producing incorrect output without any warning. The parser now throws a descriptive error immediately, e.g. `路径数据不完整：命令 "C" 缺少参数`

- **Fixed multi-line `d` attribute extraction**: paths exported by Illustrator, Inkscape, or AI tools often span multiple lines inside the `d` attribute. The previous regex `[^"']+` did not match newline characters, silently dropping entire paths. Changed to `[\s\S]*?` to handle multi-line values correctly

- **Fixed `M` command incorrectly influencing preview bounding box**: `moveto` does not render geometry and should not affect `viewBox` computation. When a path began with a distant `M`, the preview shape appeared unnecessarily scaled down

- **Fixed preview stroke becoming invisible on large-coordinate SVGs**: replaced fixed `stroke-width="1"` with `vector-effect: non-scaling-stroke` so the stroke remains visible at any coordinate scale

- **Fixed arc segments silently disappearing at near-zero angles**: added `Math.max(1, ...)` guard to `arcToBezier` segment count to prevent floating-point precision from producing zero iterations

- **Fixed background brightness slider missing accessible label association**: added `for="bgSlider"` to the `<label>` element so screen readers correctly identify the control

- **Removed unreachable dead code from precision tag map**: `\p1` and `\p3` entries were never reachable from the UI and have been removed

</details>

## v3.0

<details>
<summary><strong>中文版</strong></summary>

- **修复预览包围盒不精确的问题**：原实现以贝塞尔控制点计算 viewBox，导致图形在预览中被错误缩放或偏移；现改用参数方程求极值点，预览精度完全对齐实际图形

- **修复 `S`/`T` 命令连续使用时的 NaN 静默传播问题**：由未初始化的控制点变量引起

- **修复复制功能 API 优先级错误**：改为优先调用 `navigator.clipboard`，以废弃的 `document.execCommand` 作为降级方案

- **修复图标 MIME 类型声明错误**：`image/jpeg` → `image/png`

- **重构路径解析器 `switch` 块**：所有 `var` 改为 `let`/`const` + 显式块作用域，消除变量提升导致的跨 case 状态污染

- **Toast 计时器从 DOM 属性迁移至闭包变量**

- **新增超大输入（>500 KB）保护提示**

</details>

<details>
<summary><strong>English</strong></summary>

- **Fixed inaccurate preview bounding box**: viewBox was previously computed from Bézier control points rather than actual curve extrema, causing shapes to appear incorrectly scaled or offset in the preview. Now uses derivative roots for precise bounds

- **Fixed silent NaN propagation in `S`/`T` commands**: caused by uninitialized control point variables

- **Fixed copy button API priority**: now correctly prefers `navigator.clipboard`, with deprecated `document.execCommand` as a legacy fallback

- **Fixed icon MIME type declaration**: `image/jpeg` → `image/png`

- **Refactored path parser `switch` block**: replaced all `var` with `let`/`const` inside explicit block scopes, eliminating cross-case variable hoisting issues

- **Moved toast timer ID from a DOM property to a closure variable**

- **Added input size guard (>500 KB)** to warn users before synchronous processing may block the main thread

</details>

## v2.9

<details>
<summary><strong>中文版</strong></summary>

- 初始公开发布版本

</details>

<details>
<summary><strong>English</strong></summary>

- Initial public release

</details>


---

<div align="center">

**Made with ❤️ 用爱打造**

</div>
