# SVG to ASS Converter (v1.0)

<div align="center">

![Version](https://img.shields.io/badge/version-1.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
[![Author](https://img.shields.io/badge/author-Nick%20Collect-orange)](https://github.com/MontageSubs)

**一个现代化、高精度的 Aegisub 绘图指令转换工具**

---

## 💡 设计初衷 (Motivation)

对于字幕特效师（Typesetter）而言，将矢量图形（如 Logo、复杂的几何图案）从 Adobe Illustrator 或 Inkscape 导入 Aegisub 一直是一个繁琐的痛点。

传统的流程往往依赖于古老的插件或简陋的脚本，它们不仅安装麻烦，还经常面临版本不兼容、无法处理复杂路径、坐标精度丢失导致图形锯齿等问题。而且，当你手头只有一段杂乱的 SVG 代码时，手动清洗标签和提取坐标更是耗时耗力。

**SVG to ASS** 的诞生就是为了解决这些问题。我希望打造一个**无需安装、打开即用、现代化的 Web 工具**。它不仅能智能清洗脏数据，还能自动处理高精度缩放（避免 `\p1` 下的精度损失），并提供实时的可视化预览。

让特效制作回归创意本身，而不是浪费在枯燥的代码转换上。

## ✨ 核心特性 (Features)

* 🎨 **现代化 UI (Modern UI)**
    * 采用深色模式（Dark Mode），符合 Aegisub 用户习惯，护眼且专业。
    * 左右分栏设计，代码输入与结果预览一目了然。

* 🚀 **智能提取 (Smart Parsing)**
    * 内置“脏数据清洗器”，无需手动删除 XML 标签。
    * 支持直接粘贴整个 `<svg>` 代码块或 `<path>` 标签，自动识别并合并 `d` 属性路径。

* 🔍 **实时预览 (Real-time Preview)**
    * 左侧修改，右侧即时渲染。
    * **透明网格背景**：自带 Photoshop 风格的棋盘格，支持**亮度调节滑块**，无论黑色还是白色图形都能清晰查看。

* 📐 **高精度支持 (High Precision)**
    * 默认提供 **8x (\p4)** 和 **16x (\p5)** 两种高精度模式。
    * 通过数学算法对坐标进行预放大，完美解决 Aegisub 中小图形锯齿化的问题。

* 🛠 **自动头部 (Auto Header)**
    * 自动生成 `\fscx100\fscy100` 等基础标签，支持自定义追加 `\pos` 或 `\c` 等指令。

## 📖 如何使用 (Usage)

1.  **准备图形**：在 Adobe Illustrator / Inkscape / Figma 中设计好图形，并复制 SVG 代码 (Ctrl+C)。
2.  **粘贴**：打开本工具，将代码直接粘贴到左侧的 "Input Source" 框中。
3.  **转换**：点击 **Convert** 按钮（或直接查看预览）。
4.  **复制**：点击右侧的 **Copy** 按钮，将生成的 ASS 代码粘贴到 Aegisub 的字幕行中。

## 🛠 技术栈 (Tech Stack)

* Pure HTML5
* CSS3 (Flexbox & Grid Layout)
* Vanilla JavaScript (ES6+)
* 无需后端，纯前端运行


## 📄 许可证 (License)

本项目采用 [MIT License](LICENSE) 许可证。
你可以免费使用、修改和分发此工具。

---

**Designed & Built by [Nick Collect](https://github.com/MontageSubs)**
