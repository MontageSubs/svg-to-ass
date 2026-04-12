# SVG 转 ASS 绘图指令
**SVG to ASS Draw · MontageSubs**

<br/>

> **在浏览器中将 SVG 矢量路径一键转换为 Aegisub ASS 绘图指令，所有处理均在本地进行。**
>
> *智能转换 · 高精度*

<div align="right">

**中文 | [English](./README.en.md)**

</div><br/>

<div align="center">

| [打开工具](https://subs.js.org/svg-to-ass/) | [提交反馈](https://github.com/MontageSubs/svg-to-ass/issues) | [参与讨论](https://github.com/MontageSubs/svg-to-ass/discussions) |
| :---: | :---: | :---: |

</div><br/>

## 简介

SVG 转 ASS 绘图指令工具是由 @NickCollect 开发的开源浏览器端工具，用于将 SVG 矢量图形路径转换为 Aegisub 可识别的 ASS `\p` 绘图指令代码。

对于字幕特效师（Typesetter）而言，将矢量图形（Logo、几何图案等）从 Adobe Illustrator 或 Inkscape 导入 Aegisub 历来是一个繁琐的环节。传统流程依赖古老插件，不仅安装麻烦，还面临精度丢失、图形锯齿等问题。本工具提供一个无需安装、打开即用的现代化替代方案。

所有处理均在本地浏览器中完成，输入不离开用户设备。

## 功能

**智能路径提取与脏数据清洗**

支持直接粘贴完整的 SVG 源码，工具会自动识别并提取其中所有 `<path>` 标签的 `d` 属性，合并多条路径后统一转换，无需手动清理 XML 标签。若代码中同时存在 `<path>` 与未转曲的基础形状，工具会提取路径并给出警告。

**高精度坐标缩放**

提供 8x（`\p4`）与 16x（`\p5`）两种高精度模式。通过对坐标进行预放大，从根本上解决 Aegisub 在 `\p1` 模式下小图形因整数精度不足而产生锯齿的问题。

**实时矢量效果预览**

转换后即时在页面右侧渲染矢量形状，支持亮度调节滑块，无论图形是纯黑还是纯白都能调出最佳观察对比度。

**自动生成 ASS 头部标签**

自动添加 `\fscx1000\fscy1000` 等基础精度标签，支持自定义追加 `\pos`、`\c` 等任意行内标签，复制后可直接粘贴至 Aegisub 字幕行。

**中英双语界面**

自动跟随浏览器语言（中文环境默认中文，其余默认英文），右上角可随时手动切换，偏好跨会话保存。

## 使用方法

本工具完全基于浏览器运行，无需安装：

1. 打开 [https://subs.js.org/svg-to-ass/](https://subs.js.org/svg-to-ass/)
2. 将 SVG 代码粘贴到左侧输入框（步骤 1）
3. 在步骤 2 中选择精度并配置附加标签，点击 **转换 Convert**
4. 在右侧预览区确认效果，点击 **复制 Copy**，将代码粘贴到 Aegisub 字幕行即可

---


## 输入格式参考

工具支持以下三种输入方式：

**SVG 标准代码**

粘贴完整的 SVG 源码，支持各种图形元素（path、circle、rect 等）：

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <path d="M 50 2 C 76.5 2 98 23.5 98 50 ..."/>
  <circle cx="50" cy="50" r="40"/>
</svg>
```

**SVG Path 数据**

单条 path 标签或纯路径命令：

```svg
<path d="M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z"/>
```

```
M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z
```

**上传 SVG 文件**

直接上传 `.svg` 文件，工具自动读取处理。

---

## 精度说明

| 模式 | 缩放倍数 | ASS 标签 | 适用场景 |
|------|----------|----------|----------|
| 8x | ×8 | `\p4` | 推荐，兼顾精度与代码长度 |
| 16x | ×16 | `\p5` | 极小图形或对精度要求极高的场景 |

Aegisub 的绘图坐标为整数，`\p1` 模式下精度单位为 1 像素，容易产生锯齿。高精度模式通过预放大坐标再以对应 `\p` 级别渲染，等效提升坐标精度，使小图形边缘更平滑。

---

## 技术栈

- Pure HTML5 & CSS3
- Vanilla JavaScript (ES6+)
- 原生数学计算（贝塞尔曲线 / 椭圆弧转换）
- 无需后端，100% 纯本地浏览器处理

## 仓库结构

```
svg-to-ass/
├── index.html        # 工具主体（单文件，包含全部逻辑）
├── manifest.json     # PWA 配置 (中文)
├── manifest-en.json  # PWA 配置 (英文)
├── sw.js             # Service Worker（缓存策略）
├── LICENSE
├── README.md         # 中文说明（本文件）
└── README.en.md      # 英文说明
```

## 参与贡献

欢迎任何形式的贡献，包括但不限于：

- 在 [Issues](https://github.com/MontageSubs/svg-to-ass/issues) 中提交 Bug 报告或功能请求
- 在 [Discussions](https://github.com/MontageSubs/svg-to-ass/discussions) 中分享使用经验或技术讨论
- 提交 Pull Request 改进代码或文档

## 版本历史
### v3.3
- 支持完整 SVG 代码粘贴和文件上传
- 支持 circle、rect 等多种图形元素解析

### v3.2
- UI 界面优化
- 新增多语言支持

### v3.1
- 新增中英双语界面：自动跟随浏览器语言（中文环境默认中文，其余默认英文），右上角可随时手动切换，偏好写入 localStorage 跨会话保存
- 双语覆盖全量：UI 文字、placeholder、toast 提示、错误信息、Giscus 讨论区语言均随切换同步更新
- 采用轻量 i18n 引擎（`data-i18n` 属性 + `t()` 函数），零外部依赖，单文件架构不变

### v3.0.1
- 修复路径参数缺失时静默补零的问题：残缺路径数据（如 `C` 命令只有两个参数而非六个）原本会用 `0` 填充剩余坐标，生成错误输出却无任何提示；现改为立即抛出明确错误，如「路径数据不完整：命令 "C" 缺少参数」
- 修复多行 `d` 属性被漏提取的问题：Illustrator、Inkscape 及部分 AI 工具导出的 SVG 路径值常跨行写入，原正则 `[^"']+` 不匹配换行符导致整条路径丢失；改用 `[\s\S]*?` 后正确支持多行属性值
- 修复 `M` 命令错误纳入预览包围盒：`moveto` 不渲染几何体，不应影响 viewBox 计算；原实现在路径以远处 `M` 开头时会导致预览图形被不必要地缩小
- 修复大坐标 SVG 预览描边消失：将固定 `stroke-width="1"` 改为 `vector-effect: non-scaling-stroke`，在任意坐标尺度下描边均清晰可见
- 修复弧线在极小角度时静默消失：`arcToBezier` 中 `segments` 加入 `Math.max(1, ...)` 保护，防止浮点精度导致分段数为零
- 修复背景亮度滑块无障碍标签未关联：`<label>` 补充 `for="bgSlider"`，屏幕阅读器现可正确识别控件
- 移除精度映射表中无法命中的死代码：`\p1` 与 `\p3` 条目在 UI 中不可选，已清除

### v3.0
- 修复预览包围盒不精确的问题：原实现以贝塞尔控制点计算 viewBox，导致图形在预览中被错误缩放或偏移；现改用参数方程求极值点，预览精度完全对齐实际图形
- 修复 `S`/`T` 命令连续使用时的 NaN 静默传播问题（由未初始化的控制点变量引起）
- 修复复制功能 API 优先级错误：改为优先调用 `navigator.clipboard`，以废弃的 `document.execCommand` 作为降级方案
- 修复图标 MIME 类型声明错误（`image/jpeg` → `image/png`）
- 重构路径解析器 `switch` 块：所有 `var` 改为 `let`/`const` + 显式块作用域，消除变量提升导致的跨 case 状态污染
- Toast 计时器从 DOM 属性迁移至闭包变量
- 新增超大输入（>500 KB）保护提示

### v2.9
- 初始公开发布版本

---

## 许可证

本项目源代码遵循 [MIT License](./LICENSE) 授权。

---

<div align="center">

**蒙太奇字幕组 (MontageSubs)**  
"用爱发电 ❤️ Powered by Love"

</div>
