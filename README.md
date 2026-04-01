# SVG Renderer

基于仓颉语言（Cangjie）实现的 SVG 转 BMP 渲染器。

## 功能特性

- SVG 解析：支持 `<path>`、`<polygon>`、`<polyline>`、`<rect>`、`<circle>`、`<ellipse>`、`<line>` 等元素
- 路径命令：M/m、L/l、H/h、V/v、C/c、S/s、Q/q、T/t、A/a、Z/z
- 变换：`translate` + `scale` 组合变换，支持嵌套 `<g>` 级联
- 填充规则：`evenodd` / `nonzero`（支持继承）
- 颜色：hex 颜色值，`fill` 属性及 `style` 内联样式
- 抗锯齿：4x4 超采样（16 个子像素），贝塞尔曲线 64 步细分
- 输出：24 位 BMP 格式

## 环境要求

- 仓颉编译器 cjc 1.0.5+
- Windows 操作系统

## 编译

```bash
cjc -o out/svg_renderer.exe src/main.cj
```

## 使用

```bash
out\svg_renderer.exe <输入.svg> [输出.bmp] [宽度] [高度]
```

### 参数说明

| 参数 | 必填 | 默认值 | 说明 |
|------|------|--------|------|
| 输入 SVG | 是 | - | SVG 文件路径 |
| 输出 BMP | 否 | 与 SVG 同目录同名 | BMP 文件路径 |
| 宽度 | 否 | 按比例自动计算 | 输出图片宽度（像素） |
| 高度 | 否 | 按比例自动计算 | 输出图片高度（像素） |

### 示例

```bash
# 输出到 SVG 同目录
out\svg_renderer.exe testcase\4-cat.svg

# 指定输出路径
out\svg_renderer.exe testcase\4-cat.svg output\cat.bmp

# 指定宽高
out\svg_renderer.exe testcase\4-cat.svg output\cat.bmp 2000 2000
```

不指定宽高时，渲染器根据 viewBox 比例自动计算，保证最短边至少 1000 像素。

## 项目结构

```
├── src/
│   └── main.cj          # 主源码
├── testcase/             # 测试用例
│   ├── 2-cangjie.svg
│   ├── 2-hexagon.svg
│   ├── 4-butterfly.svg
│   ├── 4-cat.svg
│   └── 6-bird.svg
├── cjpm.toml             # 项目配置
└── README.md
```

## 已知限制

- 不支持 `stroke-width`、`stroke-linecap` 等 stroke 渲染
- 不支持渐变填充（linearGradient / radialGradient）
- 不支持透明度（opacity / fill-opacity）
- 不支持 `<text>` 文本元素
- 不支持 `<use>`、`<clipPath>`、`<mask>`
- 弧线命令（A/a）使用线性插值近似
