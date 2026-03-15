# SVG 转 ASS 绘图指令
**SVG to ASS Draw Converter · MontageSubs**

<br/>

> **在浏览器中将 SVG 矢量路径一键转换为 Aegisub ASS 绘图指令，无需安装任何软件。**
>
> *All processing runs locally in your browser. No files are ever uploaded to a server.*

<div align="right">

**中文 | [English](./README.en.md)**

</div><br/>

<div align="center">

| [打开工具](https://montagesubs.github.io/svg-to-ass/) | [提交反馈](https://github.com/MontageSubs/svg-to-ass/issues) | [参与讨论](https://github.com/MontageSubs/svg-to-ass/discussions) |
| :---: | :---: | :---: |

</div><br/>

## 简介

SVG 转 ASS 绘图指令工具是由 @NickCollect 开发的开源浏览器端工具，用于将 SVG 矢量图形路径转换为 Aegisub 可识别的 ASS `\p` 绘图指令代码。

对于字幕特效师（Typesetter）而言，将矢量图形（Logo、几何图案等）从 Adobe Illustrator 或 Inkscape 导入 Aegisub 历来是一个繁琐的环节。传统流程依赖古老插件，不仅安装麻烦，还面临精度丢失、图形锯齿等问题。本工具提供一个无需安装、打开即用的现代化替代方案。

所有处理均在本地浏览器中完成，文件不离开用户设备。

## 功能

**智能路径提取与脏数据清洗**

支持直接粘贴完整的 SVG 源码，工具会自动识别并提取其中所有 `<path>` 标签的 `d` 属性，合并多条路径后统一转换，无需手动清理 XML 标签。若代码中同时存在 `<path>` 与未转曲的基础形状，工具会提取路径并给出警告。

**高精度坐标缩放**

提供 8x（`\p4`）与 16x（`\p5`）两种高精度模式。通过对坐标进行预放大，从根本上解决 Aegisub 在 `\p1` 模式下小图形因整数精度不足而产生锯齿的问题。

**实时矢量效果预览**

转换后即时在页面右侧渲染矢量形状，支持亮度调节滑块，无论图形是纯黑还是纯白都能调出最佳观察对比度。

**自动生成 ASS 头部标签**

自动添加 `\fscx1000\fscy1000` 等基础精度标签，支持自定义追加 `\pos`、`\c` 等任意行内标签，复制后可直接粘贴至 Aegisub 字幕行。

## 使用方法

本工具完全基于浏览器运行，无需安装：

1. 打开 [https://montagesubs.github.io/svg-to-ass/](https://montagesubs.github.io/svg-to-ass/)
2. 将 SVG 代码粘贴到左侧输入框（步骤 1）
3. 在步骤 2 中选择精度并配置附加标签，点击 **转换 Convert**
4. 在右侧预览区确认效果，点击 **复制 Copy**，将代码粘贴到 Aegisub 字幕行即可

---

## ⚠️ 重要：为什么我的 SVG 代码无法转换？

这是使用本工具时最常见的问题，请先阅读本节再提交反馈。

### 根本原因

Aegisub 的 ASS 绘图指令（`\p1`）底层只理解**点和曲线的坐标序列**，不存在"圆"或"直线"这样的抽象概念。本工具的解析逻辑专门提取 SVG `<path>` 标签的 `d` 属性坐标进行换算。

SVG 格式中除 `<path>` 之外，还有一类**基础形状标签**（`<circle>`、`<rect>`、`<line>`、`<ellipse>`、`<polygon>`、`<polyline>`），它们用高层语义描述图形，而非坐标路径，**本工具无法处理这些标签**。

### 典型案例

以下是一段 AI 生成的 SVG，包含了工具无法识别的基础形状：

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <circle cx="50" cy="50" r="48"/>
  <line x1="2" y1="50" x2="98" y2="50"/>
  <line x1="50" y1="2" x2="50" y2="98"/>
</svg>
```

`<circle>` 和 `<line>` 均属于基础形状，工具无法提取路径坐标，因此无法转换。

### 解决方法

**方法一：让 AI 重新生成**

在提示词中明确要求输出路径格式：

> 请将所有图形（包括圆形、矩形、直线等）全部转换为 `<path>` 路径标签输出，不要使用 `<circle>`、`<rect>`、`<line>` 等基础形状标签。

**方法二：在矢量软件中转换后导出**

在 Adobe Illustrator、Inkscape 或 Figma 中，导出 SVG 之前请先执行以下操作：

- **Adobe Illustrator**：全选图形 → 对象 → 扩展 → 确定；或：对象 → 路径 → 轮廓化描边
- **Inkscape**：全选图形 → 路径 → 对象转路径（Object to Path）
- **Figma**：全选图形 → 右键 → Flatten Selection（拼合所选内容）

完成上述操作后再导出 SVG，所有图形将以 `<path>` 形式输出，可直接粘贴使用。

**方法三：手动打开 SVG 文件复制代码**

本工具不支持直接上传 SVG 文件。若你拥有一个 `.svg` 文件，请用文本编辑器（记事本、VS Code 等）打开它，复制其中的全部文本内容，再粘贴到输入框中。

---

## 输入格式参考

工具支持以下几种输入形式：

**完整 SVG 源码（推荐）**
```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <path d="M 50 2 C 76.5 2 98 23.5 98 50 ..."/>
  <path d="M 10 10 L 90 10 L 90 90 Z"/>
</svg>
```
工具会自动提取并合并所有 `<path>` 的 `d` 属性。

**单条 path 标签**
```svg
<path d="M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z"/>
```

**裸路径数据**
```
M 50 2 C 76.5 2 98 23.5 98 50 C 98 76.5 76.5 98 50 98 Z
```

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
├── manifest.json     # PWA 配置
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

## 许可证

本项目源代码遵循 [MIT License](./LICENSE) 授权。

---

<div align="center">

**蒙太奇字幕组 (MontageSubs)**  
"用爱发电 ❤️ Powered by Love"

</div>
