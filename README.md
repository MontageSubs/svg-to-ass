# SVG 转 ASS 绘图指令

**SVG to ASS Draw Converter · 矢量图形一键转换**

> 在浏览器中将 SVG 矢量图形转换为 ASS/SSA 字幕格式的绘图指令，所有处理均在本地进行。

<div align="right">

**中文 | [English](./README.en.md)**

</div><br/>

<div align="center">

| [打开工具](https://subs.js.org/svg-to-ass/) | [提交反馈](https://github.com/MontageSubs/svg-to-ass/issues) | [参与讨论](https://github.com/MontageSubs/svg-to-ass/discussions) |
| :---: | :---: | :---: |

</div><br/>

## 概述

**SVG 转 ASS 绘图指令工具** 是由 [@NickCollect](https://github.com/NickCollect) 开发的开源浏览器工具，用于将 SVG 矢量图形路径一键转换为 Aegisub ASS `\p` 绘图指令代码。

对于字幕特效师（Typesetter）而言，将矢量图形（Logo、几何图案、精细图标等）从 Adobe Illustrator 或 Inkscape 导入 ASS/SSA 字幕历来是一个繁琐的环节。传统流程依赖古老插件，不仅安装麻烦，还面临精度丢失、图形锯齿等问题。本工具提供一个**无需安装、打开即用**的现代化替代方案，支持高精度转换与实时预览，让复杂矢量特效制作变得简单直观。所有处理均在浏览器本地完成，输入数据不离开用户设备。

## 核心特色

- **浏览器中直接处理** — 所有转换在本地进行，支持离线使用，保护隐私，无需安装额外软件。
- **多格式输入支持** — 支持粘贴 SVG 代码、单条路径数据或上传 `.svg` 文件，智能解析 `<path>`、`<circle>`、`<rect>` 等多种图形元素。
- **高精度坐标缩放** — 提供 8x（`\p4`）与 16x（`\p5`）两种高精度模式，根本解决 ASS/SSA 字幕 `\p1` 模式下小图形因整数精度不足而产生的锯齿问题。
- **完整颜色 / 描边 / 透明度 / 模糊还原**（默认开启）— 按形状逐组发出 `\1c`（fill）、`\3c` + `\bord`（stroke + 宽度，按 `\fscx` 比例缩放）、`\1a` / `\3a`（透明度，自动反转为 ASS 习惯）、`\blur`（来自 `<feGaussianBlur>`）；正确处理 `<g>` 上的继承属性、`style="..."` 内联样式、`<style>` 中的 class 规则、`url(#id)` 渐变首个 `<stop>`；同色形状自动合并到同一段，避免多余标签切换。
- **「Flat (legacy single path)」回退** — 一个勾选项即可切回老版本的合并单 `\p1` 块、纯几何无颜色行为，方便只关心形状的下游流程；与配套工具 ass-to-svg 的同名勾选项语义完全对称。
- **原点对齐到 (0,0)** — 一个勾选项把整张图的包围盒左上角平移到原点，配合 `\an7\pos(x,y)` 即可让图形的左上角精确落在屏幕的 `(x, y)` 处，省去手动算偏移。
- **实时矢量效果预览** — 预览区直接渲染 SVG 的真实 fill / stroke / 透明度 / 模糊（与下载的 ASS 在 ASS 渲染器里看到的一致），支持亮度调节滑块。
- **一键生成 ASS 指令** — 自动添加 `\fscx1000\fscy1000` 等基础精度标签和上述颜色 / 描边 / 透明度标签，可继续追加 `\pos`、`\fs` 等行内标签，复制后可直接粘贴至 ASS/SSA 字幕。

## 功能

### 智能路径提取与脏数据清洗
粘贴或上传SVG，工具自动解析图形结构，智能识别边界和路径，精确提取矢量信息。

### 高精度坐标缩放
提供 **8x**（`\p4`）与 **16x**（`\p5`）两种高精度模式。通过对坐标进行预放大，从根本上解决 ASS/SSA 字幕 在 `\p1` 模式下小图形因整数精度不足而产生锯齿的问题，使转换后的图形边缘更平滑、更精致。

### 颜色 / 描边 / 透明度 / 模糊一并还原（默认开启）
按形状逐组发出 ASS 颜色与描边 / 透明度 / 模糊标签：
- 解析 `fill` / `stroke` / `stroke-width` / `fill-opacity` / `stroke-opacity` / `opacity` / `filter`
- 含 `<g>` 上的继承属性、`style="..."` 内联样式、`<style>` 中的简单 class 规则（如 `.cls-1 { fill: ... }`）、`url(#id)` 渐变定义的首个 `<stop>`
- 输出对应的 `\1c` / `\3c` / `\bord` / `\1a` / `\3a` / `\blur` 标签；`\bord` 与 `\blur` 自动按 `\fscx` 比例缩放（默认 `\fscx1000` ⇒ ×10）
- 同色形状自动合并为同一段，不会冗余切换

### 「Flat (legacy single path)」回退
Step 2 提供 `flatMode` 勾选项，勾上即回退到老版本的合并单 `\p1` 块行为，关闭所有颜色 / 描边 / 透明度标签输出，便于下游流程把整张图当成单一无色形状处理；勾选状态写入 `localStorage` 跨会话保留。命名与 ass-to-svg 的同名勾选项 1:1 对齐。

### 原点对齐到 (0,0)
Step 2 提供「原点对齐到 (0,0)」勾选项，勾上后整张图的包围盒左上角被平移到 `(0, 0)`。配合 `\an7\pos(x,y)`，图形的左上角就精确落在屏幕的 `(x, y)` 处，无需手动算偏移。

### 实时矢量效果预览
预览区渲染的就是 SVG 的真实 fill / stroke / 透明度 / 模糊效果（与下载到 Aegisub 后所见保持一致），并附亮度调节滑块以便核对深色图形。Flat 模式下保留原本的 accent-stroke 几何检视外观。

### 灵活的标签自定义
自动生成 `\fscx1000\fscy1000` 等基础精度标签和上述颜色 / 描边 / 透明度标签，支持继续追加 `\pos`、`\fs` 等任意行内标签。复制后可直接粘贴至 Aegisub 字幕行，无需再行调整。

### 支持多种输入格式
- 完整 SVG 源码（含 DOCTYPE 和样式）
- 单条 path 标签或纯路径命令
- 直接上传 `.svg` 文件
- **边界**：工具处理 SVG 矢量元素；嵌入式 `<image>` 位图不会被自动描摹为矢量路径。
- **特例**：当输入是无 `<svg>` 标签的纯路径数据，输出始终回退为单 `\p1` 黑色块，与 v3.11 保持兼容（无视 colors / flat 勾选）。

## 使用方法

1. 打开 [https://subs.js.org/svg-to-ass/](https://subs.js.org/svg-to-ass/)
2. 将 SVG 代码粘贴或上传到左侧输入框（步骤 1）
3. 在步骤 2 中选择精度并配置附加标签，点击 **转换**
4. 在右侧预览区确认效果，点击 **复制**，将代码粘贴到 ASS/SSA 字幕行即可

## 输入格式参考

### SVG 标准代码

粘贴完整的 SVG 源码，支持各种图形元素（path、circle、rect 等）：

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <path d="M 50 2 C 76.5 2 98 23.5 98 50 ..."/>
  <circle cx="50" cy="50" r="40"/>
</svg>
```

### SVG Path 数据

单条 path 标签或纯路径命令：

```svg
<path d="M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z"/>
```

或直接输入路径：

```
M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z
```

### 上传 SVG 文件

直接上传 `.svg` 文件，工具自动读取处理。

## 精度说明

| 模式 | 缩放倍数 | ASS 标签 | 适用场景 |
|------|----------|----------|----------|
| **8x** | ×8 | `\p4` | **推荐**，兼顾精度与代码长度 |
| **16x** | ×16 | `\p5` | 极小图形或对精度要求极高的场景 |

ASS/SSA 字幕的绘图坐标为整数，`\p1` 模式下精度单位为 1 像素，容易产生锯齿。高精度模式通过预放大坐标再以对应 `\p` 级别渲染，等效提升坐标精度，使小图形边缘更平滑。

## 技术栈

| 技术 | 说明 |
|------|------|
| **HTML5 & CSS3** | 页面结构与样式 |
| **Vanilla JavaScript (ES6+)** | 核心逻辑，无外部依赖 |
| **原生数学计算** | 贝塞尔曲线、椭圆弧转换 |
| **本地浏览器处理** | 100% 纯客户端，无后端服务 |

## 仓库结构

```
svg-to-ass/
├── app/                      # 工具主体
│   ├── index.html            # 工具主文件
│   ├── sw.js                 # Service Worker（缓存策略）
│   ├── manifests/            # PWA 配置（10 种语言）
│   ├── sitemap.xml           # 搜索引擎站点地图
│   └── icons/                # 应用图标
├── README.md                 # 中文说明（本文件）
├── README.en.md              # 英文说明
├── CHANGELOG.md              # 中英双语更新日志
└── LICENSE                   # MIT 许可证
```

## 配套工具

本工具的反向版本为 [ASS 转 SVG 矢量](https://subs.js.org/ass-to-svg/)。两个工具 feature set 已对称（默认都开颜色还原、都提供同名 `Flat (legacy single path)` 回退勾选项），可以实现 SVG ↔ ASS 的双向转换工作流。

## 本地化

本工具完整支持**中文和英文**，并支持西班牙语、葡萄牙语、俄语、日语、韩语、阿拉伯语、土耳其语等多种语言。

如果发现翻译错误或想帮助改进其他语言，欢迎在 [Issues](https://github.com/MontageSubs/svg-to-ass/issues) 或 [Discussions](https://github.com/MontageSubs/svg-to-ass/discussions) 中提出建议。我们欢迎社区贡献！

## 参与贡献

欢迎任何形式的贡献，包括但不限于：

- **功能开发** — 新功能实现、bug 修复、性能优化、兼容性改进
- **文档完善** — 改进 README、补充使用指南、编写教程与最佳实践
- **国际化** — 翻译改进、语言支持扩展、本地化优化
- **反馈建议** — 提交 bug 报告、功能建议、用户体验反馈
- **推广分享** — 向朋友推荐、分享到社交媒体、撰写使用心得

欢迎在 [Issues](https://github.com/MontageSubs/svg-to-ass/issues) 和 [Discussions](https://github.com/MontageSubs/svg-to-ass/discussions) 中参与讨论。加入我们吧！

### 贡献者

<details>
<summary><strong>核心团队</strong></summary>

- **NickCollect** ([@NickCollect](https://github.com/NickCollect)) — 项目首席开发者，核心算法与前端交互设计
- **小p** ([@mtsubs](https://github.com/mtsubs)) — 前端 UI 开发

</details>

## 许可证

本项目源代码遵循 [MIT License](./LICENSE) 授权。

---

<div align="center">

**蒙太奇字幕组 (MontageSubs)**  
"用爱发电 ❤️ Powered by Love"

</div>

