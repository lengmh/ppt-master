# {project_name} - 设计规格

> 本文件是给人阅读的设计叙述，用来说明方案依据、受众、风格、配色与内容大纲。下游角色会读取一次，用于建立上下文理解。
>
> 机器可执行的约束写在 `spec_lock.md` 中（其中包含颜色 / 字体 / 图标 / 图片等的简化执行合同）。Executor 在生成每一页 SVG 前都会重新读取 `spec_lock.md`，以避免长上下文中的信息漂移。两者必须保持同步；如果出现不一致，以 `spec_lock.md` 为准。

## I. 项目信息

| 项目 | 值 |
| ---- | ---- |
| **项目名称** | {project_name} |
| **画布格式** | {canvas_info['name']} ({canvas_info['dimensions']}) |
| **页数** | [由 Strategist 填写] |
| **设计风格** | {design_style} |
| **目标受众** | [由 Strategist 填写] |
| **使用场景** | [由 Strategist 填写] |
| **创建日期** | {date_str} |

---

## II. 画布规格

| 属性 | 值 |
| ---- | ---- |
| **格式** | {canvas_info['name']} |
| **尺寸** | {canvas_info['dimensions']} |
| **viewBox** | `{canvas_info['viewbox']}` |
| **边距** | [由 Strategist 推荐，例如：左右 60px，上下 50px] |
| **内容区域** | [根据画布计算] |

---

## III. 视觉主题

### 主题风格

- **Style**: {design_style}
- **Theme**: [浅色主题 / 深色主题]
- **Tone**: [由 Strategist 填写，例如：科技、专业、现代、创新]

### 配色方案

> Strategist 应根据项目内容、行业属性和品牌色来确定具体颜色值。

| 角色 | HEX | 用途 |
| ---- | --- | ---- |
| **背景色** | `#......` | 页面背景（浅色主题通常为白色；深色主题通常为深灰或深蓝） |
| **次级背景** | `#......` | 卡片背景、区块背景 |
| **主色** | `#......` | 标题装饰、关键区块、图标 |
| **强调色** | `#......` | 数据高亮、关键信息、链接 |
| **次强调色** | `#......` | 次级强调、渐变过渡 |
| **正文文字** | `#......` | 正文主文字（深色主题下通常为浅色文字） |
| **次级文字** | `#......` | 注释、说明文字 |
| **三级文字** | `#......` | 补充信息、页脚 |
| **边框 / 分隔线** | `#......` | 卡片边框、分隔线 |
| **成功色** | `#......` | 正向指标（绿色系） |
| **警示色** | `#......` | 问题标记（红色系） |

> **参考**：行业配色可参考 `references/strategist.md` 或 `scripts/config.py` 中的 `INDUSTRY_COLORS`。

### 渐变方案（如需要，使用 SVG 语法）

```xml
<!-- 标题渐变 -->
<linearGradient id="titleGradient" x1="0%" y1="0%" x2="100%" y2="100%">
  <stop offset="0%" stop-color="#[primary]"/>
  <stop offset="100%" stop-color="#[secondary accent]"/>
</linearGradient>

<!-- 背景装饰渐变（注意：禁止 rgba，请使用 stop-opacity） -->
<radialGradient id="bgDecor" cx="80%" cy="20%" r="50%">
  <stop offset="0%" stop-color="#[primary]" stop-opacity="0.15"/>
  <stop offset="100%" stop-color="#[primary]" stop-opacity="0"/>
</radialGradient>
```

---

## IV. 字体系统

### 字体方案

> **按角色区分字体是预期行为，不是可选项。** Title / Body / Emphasis / Code 可以分别使用不同字体。例如：标题使用展示型衬线体，正文使用几何无衬线体。一套稿件不要求全篇只用一种字体。可先参考 [strategist.md §g — Font Combinations](../references/strategist.md) 中的常见组合，也可以提出新的搭配。
>
> **⚠️ PPT 安全字体栈规则（硬性约束）**。PPTX 对每一段文字只保存单个 `typeface`，不存在运行时回退。因此下面每个字体栈都 **必须** 以跨平台预装字体结尾：(`"Microsoft YaHei", sans-serif` / `SimSun, serif` / `Arial, sans-serif` / `"Times New Roman", serif` / `Consolas, "Courier New", monospace`)。如果字体栈前面使用了非预装字体（如 Inter / Google Fonts / 品牌专用字体），则必须在本规格中明确说明需要安装或嵌入该字体。

**Typography direction**: [填写一句概括，例如：`modern CJK sans` / `academic serif` / `brand-specific: McKinsey Bower (requires font install)`]

同一组字体决策需要从两个视角同时填写，并保持一致：

- **角色拆解表**（下表）—— 列出每个角色用到的“部件”：中文字体、英文字体、CSS 通用回退。这是给人看的设计语言。
- **按角色的字体栈**（表后）—— 列出真正写入 SVG `font-family=""` 与 `spec_lock.md` 的有序 CSS `font-family` 字符串。顺序很重要，因为它决定浏览器如何渲染字符（Latin-led vs. CJK-led）。这一部分是**实际数据**，不能仅靠上表推导。

| 角色 | 中文 | 英文 | 回退尾部 |
| ---- | ---- | ---- | -------- |
| **Title** | [例如：`"Microsoft YaHei"`，或为了兼顾 macOS 预览写成 `"Microsoft YaHei", "PingFang SC"`] | [例如：`Georgia`] | [例如：`serif`] |
| **Body** | [例如：`"Microsoft YaHei", "PingFang SC"`] | [例如：`Arial`] | [例如：`sans-serif`] |
| **Emphasis** | [例如：`SimSun`，或如果仅用于英文可写 `—`] | [例如：`Georgia`] | [例如：`serif`] |
| **Code** | — | [例如：`Consolas, "Courier New"`] | [例如：`monospace`] |

**按角色的字体栈**（CSS `font-family` 字符串，每个角色一行——按照你希望的显示顺序填写上表中的字体）：

- Title: `[填写字体栈，例如：Georgia, "Microsoft YaHei", serif 表示 Latin-led；或 "Microsoft YaHei", "PingFang SC", Georgia, serif 表示 CJK-led]`
- Body: `[填写字体栈 —— 可以与 Title 相同]`
- Emphasis: `[填写字体栈，或写成 "same as Body" 表示不单独覆盖]`
- Code: `[填写等宽字体栈，例如：Consolas, "Courier New", monospace]`

> **为什么字体栈顺序重要**：CSS `font-family` 是按字体逐个回退的，而不是所有字体并行逐字挑选。浏览器会先使用**第一个已安装字体**渲染所有它能渲染的字符，只有遇到缺字时才会跳到下一个字体。因此：
> - `Georgia, "Microsoft YaHei", serif` → 拉丁字母会优先用 Georgia（更有衬线质感），中文字符再回退到 Microsoft YaHei。**适用于以拉丁文字风格为主要设计表达的场景**（学术风 / 编辑风 / 封面英文占比较高）。
> - `"Microsoft YaHei", Georgia, serif` → 几乎所有字符都会优先用 Microsoft YaHei（包括英文，英文也会使用雅黑自带字形），整体观感会不同。**适用于中文为主、英文只是附带信息的稿件**。
>
> 转换器（`drawingml_utils.py parse_font_family`）在导出 PPTX 时会分别映射到 `<a:latin>` / `<a:ea>` 字体，但浏览器预览和 SVG 原生显示仍然会体现字体栈顺序，所以要根据你的设计意图来排顺序。

> **为什么要保留这两个视角**：角色拆解表便于快速理解每类文字的字体分配；而字体栈保存了顺序信息，这是角色拆解表表达不了的。两者必须保持一致——表格中出现的字体，应该与字体栈里使用的字体一一对应（顺序可不同）。

### 字号层级

> **这是一个比例坡度（ramp），不是固定菜单。** `body` 正文字号是唯一的锚点；其他字号都相对它按比例推导。下表定义的是**比例范围**：Executor 可以在生成时自由选取处于该范围内的字号（例如 40px 的 hero number、13px 的图表注释、72px 的封面标题），不需要事先在 `spec_lock.md` 中穷举所有字号。
> **单位约定**：统一使用 px（SVG 原生单位），避免 pt / px 转换误差。
> **基线选择原则**：应由**内容密度**决定，而不是由风格标签决定。

**Baseline**: Body font size = [填写]px（任意合理整数都可以——最常见的是 `18` 和 `24`，但图表很密时用 `16`、中等密度可用 `20` / `22`、海报或封面型稿件可用 `28-32`。核心原则是：看内容密度，不看风格名。）

| 用途 | 相对 body 的比例 | body=24（宽松）示例 | body=18（紧凑）示例 | 字重 |
| ---- | ---------------- | ------------------ | ------------------ | ---- |
| 封面标题（hero headline） | 2.5-5x | 60-120px | 45-90px | Bold / Heavy |
| 章节 / 分节页标题 | 2-2.5x | 48-60px | 36-45px | Bold |
| 页面标题 | 1.5-2x | 36-48px | 27-36px | Bold |
| Hero 数字（咨询风 KPI） | 1.5-2x | 36-48px | 27-36px | Bold |
| 副标题 | 1.2-1.5x | 29-36px | 22-27px | SemiBold |
| **正文内容** | **1x** | **24px** | **18px** | Regular |
| 注释 / 说明文字 | 0.7-0.85x | 17-20px | 13-15px | Regular |
| 页码 / 页脚注释 | 0.5-0.65x | 12-16px | 9-12px | Regular |

> 上表中的两列 px 数值只是最常见基线的演示。若你使用其他 `body` 值，直接按比例计算即可——检查器（`svg_quality_checker._check_spec_lock_drift`）会从 `spec_lock.md` 读取实时的 `body` 并按比例带校验，无需改代码。

> 如果某个字号不落在**任何**比例带内，则默认禁止使用。此时应在 `spec_lock.md typography` 中扩展角色型字号（例如 `cover_title: 96`），而不是临时发明一个一次性值。

---

## V. 布局原则

### 页面结构

- **Header area**: [填写高度和内容说明]
- **Content area**: [填写高度和内容说明]
- **Footer area**: [填写高度和内容说明]

### 布局模式库（可组合，也可按内容打破）

> **原则——比例由信息权重决定，而不是由预设比例决定。** 下表是一个**模式库**，不是菜单。Executor 可以在同一页组合两种模式、为 `breathing` 页面完全打破栅格，或者在内容确实需要时提出不在表中的新模式。每一页都默认做成对称卡片网格，正是最容易产生“AI 味”的原因——要有意识地变化。

| 模式 | 适用场景 |
| ---- | -------- |
| **Single column centered** | 封面、结论页、关键观点页 |
| **Symmetric split (5:5)** | 双边权重相当的对比页 |
| **Asymmetric split (3:7 / 2:8)** | 一侧明显更重——如图表 + 简短结论、图片 + 说明 |
| **Top-bottom split** | 流程、时间线、超宽图片 + 文本 |
| **Three/four column cards** | 特征列表、并列观点、团队介绍 |
| **Matrix grid (2×2)** | 双轴分类、战略象限 |
| **Z-pattern / waterfall** | 讲故事、案例页——内容左右交错，带动视线流动 |
| **Center-radiating** | 核心概念 + 周围节点、生态 / 利益相关者图 |
| **Full-bleed + floating text** | `breathing` / 展示型页面——图片铺满画布，文字通过透明层浮在其上 |
| **Figure-text overlap** | 强调型页面——标题或大数字压在图像边缘或图形之上，而不是并排摆放 |
| **Negative-space-driven** | 单一元素放在 40-60% 留白中，让一个观点更有重量 |

### 间距规范

> 间距默认值取决于**容器类型**。卡片只是其中一种容器，不是全局默认。下面按容器类型分别给出建议；一页可以只使用其中一组（例如没有卡片的 `breathing` 页，只需要看 universal 和 non-card 部分）。

**Universal**（适用于任意容器类型）：

| 元素 | 建议范围 | 当前项目 |
| ---- | -------- | -------- |
| 距离画布边缘的安全边距 | 40-60px | [填写] |
| 内容块之间的间距 | 24-40px | [填写] |
| 图标与文字间距 | 8-16px | [填写] |

**Card-based layouts**（仅当页面使用卡片时参考——通常是 `dense` 页的并列容器布局）：

| 元素 | 建议范围 | 当前项目 |
| ---- | -------- | -------- |
| 卡片间距 | 20-32px | [填写] |
| 卡片内边距 | 20-32px | [填写] |
| 卡片圆角半径 | 8-16px | [填写] |
| 单行卡片高度 | 530-600px | [填写] |
| 双行卡片高度 | 265-295px each | [填写] |
| 三列卡片宽度 | 360-380px each | [填写] |

**Non-card containers**（裸文本块 / 全幅图像 / 分隔线式内容——常用于 `breathing` 页或极简页面）：

- 区块之间的垂直节奏主要依靠**留白**来维持，而不是依靠卡片外框或栅格槽位。
- **Line-height (leading)**: 1.4-1.6× body font size —— 采用标准排版比例即可。
- **全幅图文字摆放**：文字应避开图像的视觉焦点区域；如果文字叠在照片上，通常需要加渐变或透明遮罩层保证可读性。
- **内容宽度**应由阅读舒适度和图像构图决定，而不是反推一个“列宽”；当页面本身没有列结构时，不要强行把它塞回栅格思维。

---

## VI. 图标使用规范

### 来源

- **内置图标库**：`templates/icons/`（三个库合计 6700+ 图标）
- **使用方式**：占位符格式 `{{icon:category/icon-name}}`

### 推荐图标列表（按需填写）

| 用途 | 图标路径 | 页面 |
| ---- | -------- | ---- |
| [示例] | `{{icon:interface/check-circle}}` | Slide XX |

---

## VII. 可视化参考清单（如需要）

> 当演示稿中包含数据可视化或信息图式结构化表达时，Strategist 应从 `templates/charts/charts_index.json` 中选择可视化类型，并在这里列出，供 Executor 参考。路径仍保留在 `templates/charts/` 下，以兼容旧流程。

| 可视化类型 | 参考模板 | 使用页面 |
| ---------- | -------- | -------- |
| [例如：grouped_bar_chart] | `templates/charts/grouped_bar_chart.svg` | Slide 05 |

---

## VIII. 图片资源清单（如需要）

| 文件名 | 尺寸 | 比例 | 用途 | 类型 | 状态 | 生成说明 |
| ------ | ---- | ---- | ---- | ---- | ---- | -------- |
| cover_bg.png | {canvas_info['dimensions']} | [ratio] | 封面背景 | [Background/Photography/Illustration/Diagram/Decorative] | [Pending/Existing/Placeholder] | [AI generation prompt] |

**状态说明**：

- **Pending** - 需要 AI 生成，请提供详细描述
- **Existing** - 用户已提供图片，放入 `images/`
- **Placeholder** - 尚未处理，SVG 中先用虚线框占位

**类型说明**（供 Image_Generator 选择提示词策略）：

- **Background** - 用于封面 / 章节页的全页背景，需预留文本区域
- **Photography** - 真实场景、人物、产品、建筑等
- **Illustration** - 扁平插画、矢量风格、卡通、概念图
- **Diagram** - 流程图、架构图、概念关系图
- **Decorative** - 局部装饰、纹理、边框、分隔线

---

## IX. 内容大纲

### Part 1: [章节名称]

#### Slide 01 - 封面

- **Layout**: 全屏背景图 + 居中标题
- **Title**: [主标题]
- **Subtitle**: [副标题]
- **Info**: [作者 / 日期 / 机构]

#### Slide 02 - [页面名称]

- **Layout**: [从 §V 中选择一种模式、组合两种模式，或按内容需要打破栅格]
- **Title**: [页面标题]
- **Visualization**: [visualization_type]（见 VII. Visualization Reference List）
- **Content**:
  - [要点 1]
  - [要点 2]
  - [要点 3]

> **Visualization 字段说明**：只有当页面确实包含数据可视化或结构化信息图元素时才填写；填写的可视化类型必须已经在 VII 节中列出。

---

[Strategist 根据源材料内容与页数规划，继续补充后续页面……]

---

## X. 演讲备注要求

为每一页生成对应的 speaker notes 文件，并保存到 `notes/` 目录：

- **File naming**: 与 SVG 文件名一致，例如 `01_cover.md`
- **Content includes**: 讲述重点、时间提示、过渡语

---

## XI. 技术约束提醒

### SVG Generation Must Follow:

1. viewBox: `{canvas_info['viewbox']}`
2. 背景使用 `<rect>` 元素
3. 文字换行使用 `<tspan>`（禁止使用 `<foreignObject>`）
4. 透明度使用 `fill-opacity` / `stroke-opacity`；禁止 `rgba()`
5. 禁止：`mask`, `<style>`, `class`, `foreignObject`
6. 禁止：`textPath`, `animate*`, `script`
7. `marker-start` / `marker-end` 可条件使用：`<marker>` 必须写在 `<defs>` 中，`orient="auto"`，图形必须是三角形 / 菱形 / 圆形（详见 `shared-standards.md` §1.1）
8. `clipPath` **仅允许用于 `<image>` 元素**：`<clipPath>` 必须写在 `<defs>` 中，且只包含一个形状子元素（circle / ellipse / 带 rx,ry 的 rect / path / polygon）。不要把它应用在 shape / group / text 上——如果要做对应形状，直接用原生元素（`<circle>` / `<ellipse>` / `<rect rx>` / `<polygon>` / `<path>`）绘制即可。详见 `shared-standards.md` §1.2

### PPT Compatibility Rules:

- 禁止 `<g opacity="...">`（组透明度）；应分别设置到每个子元素上
- 图片透明度应通过叠加遮罩层实现（`<rect fill="bg-color" opacity="0.x"/>`）
- 仅允许内联样式；禁止外部 CSS 和 `@font-face`
