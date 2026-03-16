### v3.0.1
- Fixed silent zero-fill on missing path arguments: truncated path data (e.g. a `C` command with two arguments instead of six) previously filled missing values with `0`, producing incorrect output without any warning. The parser now throws a descriptive error immediately, e.g. `路径数据不完整：命令 "C" 缺少参数`
- Fixed multi-line `d` attribute extraction: paths exported by Illustrator, Inkscape, or AI tools often span multiple lines inside the `d` attribute. The previous regex `[^"']+` did not match newline characters, silently dropping entire paths. Changed to `[\s\S]*?` to handle multi-line values correctly
- Fixed `M` command incorrectly influencing preview bounding box: `moveto` does not render geometry and should not affect `viewBox` computation. When a path began with a distant `M`, the preview shape appeared unnecessarily scaled down
- Fixed preview stroke becoming invisible on large-coordinate SVGs: replaced fixed `stroke-width="1"` with `vector-effect: non-scaling-stroke` so the stroke remains visible at any coordinate scale
- Fixed arc segments silently disappearing at near-zero angles: added `Math.max(1, ...)` guard to `arcToBezier` segment count to prevent floating-point precision from producing zero iterations
- Fixed background brightness slider missing accessible label association: added `for="bgSlider"` to the `<label>` element so screen readers correctly identify the control
- Removed unreachable dead code from precision tag map: `\p1` and `\p3` entries were never reachable from the UI and have been removed
