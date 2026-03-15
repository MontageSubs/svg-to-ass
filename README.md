# SVG to ASS Converter (v2.3)

<div align="center">

![Version](https://img.shields.io/badge/version-2.3-blue)
![License](https://img.shields.io/badge/license-MIT-green)
[![Author](https://img.shields.io/badge/author-NickCollect-orange)](https://github.com/MontageSubs)

**一个现代化、高精度的 Aegisub 绘图指令在线转换工具**

---

## 💡 设计初衷 (Motivation)

对于字幕特效师（Typesetter）而言，将矢量图形（如 Logo、复杂的几何图案）从 Adobe Illustrator 或 Inkscape 导入 Aegisub 一直是一个繁琐的痛点。

传统的流程往往依赖于古老的插件或简陋的脚本，它们不仅安装麻烦，还经常面临版本不兼容、无法处理复杂路径、坐标精度丢失导致图形锯齿等问题。而且，当你手头只有一段杂乱的 SVG 代码时，手动清洗标签和提取坐标更是耗时耗力。

**SVG to ASS** 的诞生就是为了解决这些问题。我希望打造一个**无需安装、打开即用、现代化的 Web 工具**。它不仅能智能清洗脏数据，还能自动处理高精度缩放（避免 `\p1` 下的精度损失），并提供实时的可视化预览。

让特效制作回归创意本身，而不是浪费在枯燥的代码转换上。

## ✨ 核心特性 (Features)

* 🎨 **现代化响应式 UI (Modern Responsive UI)**
    * **自适应明暗主题**（Light/Dark Mode），跟随系统设置自动切换，提供统一且舒适的沉浸式体验。
    * 采用**自适应双栏布局（Grid Layout）**，宽屏下代码输入与结果预览左右对照、一目了然；移动端自动优雅折叠为垂直流。

* 🚀 **智能提取与防呆 (Smart Parsing & Validation)**
    * 内置“脏数据清洗器”，无需手动删除 XML 标签，自动识别并合并 `d` 属性路径。
    * **强大的错误拦截**：自动检测 `<circle>`、`<rect>` 等未转曲的基础形状，精准拦截报错并提供修改引导，彻底告别“假死”和无效转换。

* 🔍 **实时预览 (Real-time Preview)**
    * 转换后即时渲染矢量形状。
    * **自定义透明网格背景**：支持**亮度调节滑块**，无论导入的是纯黑还是纯白图形，都能调节出最佳的观察对比度。

* 📐 **高精度支持 (High Precision)**
    * 默认提供 **8x (\p4)** 和 **16x (\p5)** 两种高精度模式。
    * 通过数学算法对坐标进行预放大，完美解决 Aegisub 中小图形锯齿化的问题。

* 🛠 **自动头部 (Auto Header)**
    * 自动生成 `\fscx1000\fscy1000` 等基础精度标签，支持自定义追加 `\pos` 或 `\c` 等指令。

## 📖 如何使用 (Usage)

1.  **准备图形**：在 Adobe Illustrator / Inkscape / Figma 中设计好图形，**确保执行“轮廓化描边”或“对象转为路径”**，然后复制 SVG 代码。
2.  **输入源文件**：将代码直接粘贴到左侧 **[步骤 1]** 的文本框中。
3.  **配置与转换**：在左下角 **[步骤 2]** 中选择你需要的精度（推荐 8x），点击 **Convert** 按钮。
4.  **预览与复制**：在右侧 **[步骤 3]** 中检查渲染效果，确认无误后点击 **Copy**，将生成的 ASS 代码粘贴到 Aegisub 的字幕行中即可。

## 🛠 技术栈 (Tech Stack)

* Pure HTML5 & CSS3
* Vanilla JavaScript (ES6+)
* 原生 API 数学计算（贝塞尔曲线转换）
* 无需后端，100% 纯本地浏览器处理

## 📄 许可证 (License)

本项目采用 [MIT License](LICENSE) 许可证。
你可以免费使用、修改和分发此工具。

---

**Designed & Built by [NickCollect](https://github.com/MontageSubs)**
