# 执行锁

> **⚠️ 本文件是给 Strategist 使用的参考骨架 —— 不要原样拷贝到项目里。** 当你生成 `<project_path>/spec_lock.md` 时，只保留结构性的 `##` 分节和填好的 `-` 数据行。不要把本文件中的 `>` 引导说明、提示框、硬性规则说明、或 `> ```code fence` 里的覆盖示例一并带入项目文件——这些内容是给 Strategist 写作时参考的，不是运行时数据。最终输出必须是机器可解析的执行合同；每一行都必须是可解析数据。
>
> 这是机器可读的执行合同。Executor 在生成每一页 SVG 前都 **必须** 读取它。凡是这里没有列出的值，都 **不得** 出现在 SVG 中。设计叙述（动机、受众、风格）请写在 `design_spec.md`。
>
> 一旦开始生成 SVG，这个文件就成为颜色 / 字体 / 图标 / 图片值的唯一权威来源。后续如需修改，应通过 `scripts/update_spec.py` 同步更新本文件和已生成的 SVG，避免两边漂移。

## canvas
- viewBox: 0 0 1280 720
- format: PPT 16:9

> Strategist：请根据选定画布填写 viewBox 和 format。常见值包括：`0 0 1280 720`（PPT 16:9）、`0 0 1024 768`（PPT 4:3）、`0 0 1242 1660`（小红书）、`0 0 1080 1080`（朋友圈）、`0 0 1080 1920`（Story）。

## colors
- bg: #FFFFFF
- primary: #......
- accent: #......
- secondary_accent: #......
- text: #......
- text_secondary: #......
- border: #......

> Strategist：只填写这份稿件中**实际会使用**的颜色。可以按需增加行；未使用的颜色不要保留成 `#......`，应直接删除。

## typography
- font_family: "Microsoft YaHei", Arial, sans-serif
- title_family: Georgia, SimSun, serif
- body_family: "Microsoft YaHei", "PingFang SC", Arial, sans-serif
- emphasis_family: Georgia, SimSun, serif
- code_family: Consolas, "Courier New", monospace
- body: 22
- title: 32
- subtitle: 24
- annotation: 14

> **这里显式列出五条 family 行**，是为了让 Strategist 在填写时不会漏掉 `code_family` 或 `emphasis_family`。在真实项目的 `spec_lock.md` 中：
> - 如果某个 `*_family` 与 `font_family` 确实不同，就保留它；
> - 如果某个 `*_family` 与 `font_family` 完全相同，则应**省略**该行——Executor 对缺失角色会自动回退到 `font_family`，重复写一遍只是噪音。（例外：`code_family` 即使相同也建议保留，因为它代表概念上独立的等宽角色。）
>
> `font_family` 是默认回退字体；所有声明出来的 family 都必须是 CSS 字体栈字符串。
>
> **这些字符串来自哪里**：它们应直接复制自 `design_spec.md §IV Font Plan` 中的 *Per-role font stacks* 列表。`design_spec.md` 表格里的（Chinese / English / Fallback）列是便于人理解的辅助视图；真正的有序 CSS 栈写在这里（以及 `design_spec.md` 的同一部分），两边必须逐字符一致。字体栈的**顺序**承载着浏览器渲染意图（Latin-led vs. CJK-led），而这个信息是表格本身表达不出来的，详见 `design_spec.md §IV` 中的说明。
>
> 字号（`body` / `title` / 等）统一使用 px，与 SVG 原生单位一致。`body` 是**必须存在的基线锚点**——整份稿件中的其他字号都以它为比例推导（参见 `design_spec_reference.md §IV` 中的 ramp table）。
>
> **这些字号槽位是锚点，不是封闭菜单。** 常见槽位（`title` / `subtitle` / `annotation`）覆盖了高频情况。如果某份稿件确实需要角色型字号（如 `cover_title: 72`、`hero_number: 48`、`chart_annotation: 13`），应主动添加——封面型稿件、咨询风大数字页、信息密度很高的页面都经常需要。只要某个字号与 `body` 的比例仍落在对应角色的允许范围内，Executor 也可以使用中间值。
>
> **⚠️ PPT 安全字体栈规则（硬性约束）。** PPTX 每个 text run 只保存一个 `typeface`，没有运行时回退。所以这里每一条字体栈都 **必须** 以跨平台预装字体收尾：`"Microsoft YaHei", sans-serif` / `SimSun, serif` / `Arial, sans-serif` / `"Times New Roman", serif` / `Consolas, "Courier New", monospace`。如果前面用了非预装字体（如 Inter / Google Fonts / 品牌字体），只有在 Design Spec 明确写了“需要安装或嵌入该字体”时才允许。
>
> **字体栈长度规则。** 3-4 个字体是最合适的。转换器在写入 PPTX 时只会保留**第一个** Latin 字体和**第一个** CJK 字体，后面的项会被静默丢弃。macOS 独占字体（如 `Songti SC`、`Menlo`、`Monaco`、`Helvetica`）会通过 `FONT_FALLBACK_WIN` 自动映射到对应的 Windows 字体（见 `scripts/svg_to_pptx/drawingml_utils.py`），所以没有必要在同一个栈里同时写 macOS 字体和它的 Windows 等价物。建议优先以 Windows 预装字体作为主字体（`Microsoft YaHei` / `SimSun` / `Arial` / `Georgia` / `Consolas`），最多再保留 **一个** macOS 专属字体（通常是 `"PingFang SC"`）作为浏览器预览时的兼容优化。

## icons
- library: chunk
- inventory: target, bolt, shield, users, chart-bar, lightbulb

> `library` 必须是 `chunk` / `tabler-filled` / `tabler-outline` 三者之一（且只能选一个，禁止混用）。`inventory` 列出允许使用的图标名（不带库前缀）；Executor 只能从这份清单中选图标。

## images
- cover_bg: images/cover_bg.jpg

> 每张实际会用到的图片都单独占一行。如果整份稿件不使用图片，就把整个 `images` 分节删掉。

## page_rhythm
- P01: anchor
- P02: dense
- P03: breathing
- P04: dense
- P05: dense
- P06: breathing
- P07: anchor

> 每页一行。键格式固定为 `P<NN>`（两位补零，对应 `design_spec.md` 的 `§IX Content Outline` 页码顺序）。值只能是下面三种 rhythm tag 之一。这个字段的存在是为了打破“每一页都长一样”的问题——Executor 会逐页读取它，并按标签应用相应布局纪律。
>
> **词汇表**（只能使用这三个值）：
> - `anchor` —— 结构页（封面 / 章节开场 / 目录 / 结束页）。按对应模板处理即可。
> - `dense` —— 信息密度高的页面（数据、KPI、对比、多点列表）。允许卡片网格、多列布局、表格与图表。
> - `breathing` —— 低密度页面（单一概念、hero quote、大图 + 说明、章节过渡）。要避免把**多卡片并列网格**作为主内容结构；应优先使用裸文本、分隔线、留白或全幅图像来组织页面。单个圆角强调块、标签、callout、圆角主图都可以。整体比例由信息权重决定，而不是照搬某种固定比例菜单。
>
> **节奏服从叙事**（给 Strategist 填写这一节时参考）：只有在叙事真的需要停顿时，才安排 `breathing` 页面——例如章节切换、一个值得单页强调的判断、或一串 dense 页面后的刻意停顿。高密度数据汇报或咨询分析稿完全可能几乎全是 `dense` —— **不要为了“看起来有节奏”而硬塞 filler page**。校验标准是：每一张 `breathing` 页都必须能回答“这一页单独想说的是什么？”
>
> **缺失或留空整个分节** → Executor 会默认把所有页当作 `dense`（也就是当前旧行为）。只有兼容老项目时才允许省略；新写的稿件必须填写这一节。

## forbidden
- Mixing icon libraries
- rgba()
- `<style>`, `class`, `<foreignObject>`, `textPath`, `@font-face`, `<animate*>`, `<script>`, `<iframe>`, `<symbol>`+`<use>`
- `<g opacity>` (set opacity on each child element individually)
