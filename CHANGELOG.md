# Changelog | 更新日志

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

