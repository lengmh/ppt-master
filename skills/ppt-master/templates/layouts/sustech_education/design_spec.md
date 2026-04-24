# SUSTech Education - 设计规格

> 面向课程讲授、教学汇报、学术课程分享与方法型内容讲解的 SUSTech 教学模板。模板保留学校品牌识别，并在封面、目录、章节、结束页继承校园视觉资产，在内容页增强教学可读性、提纲导航与高密度知识表达。

---

## I. 项目信息

| 项目 | 值 |
| ---- | ---- |
| **项目名称** | SUSTech Education |
| **画布格式** | Standard 16:9 (1280 × 720) |
| **页数** | 4 个核心模板页 + 1 个可选目录页 |
| **设计风格** | 教学导向 · 学术清爽 · 轻科技 · 低干扰 |
| **目标受众** | 本科/研究生课堂、课程汇报、科研训练、方法分享 |
| **使用场景** | 课程讲义、教学汇报、课堂答辩、案例分析、技术授课 |
| **创建日期** | 2026-04-24 |

### 参考依据

1. 参考 PPTX：`神经网络驱动的射频耦合器设计.pptx`
2. 读取的代表页索引：1、2、3、10、17、24、31、38、45、52
3. 保留了参考稿中最稳定的品牌语法：左上双色斜切标签、顶部分隔线、右上角校徽横标、白底教学页结构
4. 对参考稿中大量一次性内容型图表与复杂局部装饰做了模板化简化，优先保证教学复用性与后续自动生成时的版式稳定性

---

## II. 画布规格

| 属性 | 值 |
| ---- | ---- |
| **格式** | Standard 16:9 |
| **尺寸** | 1280 × 720 |
| **viewBox** | `0 0 1280 720` |
| **边距** | 左右 72px，上 44px，下 32px |
| **内容安全区** | `x: 72–1208`, `y: 118–628` |
| **页眉高度** | 92px |
| **页脚基线** | `y = 706` |

---

## III. 视觉主题

### 主题风格

- **Style**: SUSTech 教学模板
- **Theme**: 浅色主题
- **Tone**: 清晰、克制、结构化、适合长篇知识讲授

### 配色方案

| 角色 | HEX | 用途 |
| ---- | --- | ---- |
| **背景色** | `#FFFFFF` | 全页主背景 |
| **次级背景** | `#F7F9FB` | 卡片、辅助区块、轻提示带 |
| **主色** | `#003F43` | 标题、页眉识别、关键结构线 |
| **强调色** | `#E3660D` | 标签、编号、重点强调 |
| **次强调色** | `#01ABA8` | 辅助强调、结构标识 |
| **辅助蓝** | `#4472C4` | 教学图示、表格/流程补充色 |
| **正文文字** | `#1D1C1C` | 正文主文字 |
| **次级文字** | `#5C5C5C` | 副标题、说明文字 |
| **三级文字** | `#9D9D9D` | 页脚、辅助标签 |
| **边框 / 分隔线** | `#CECECE` | 页眉页脚线、卡片边框、参考线 |
| **成功色** | `#70AD47` | 正向提示、流程完成态 |
| **警示色** | `#C00000` | 风险、警示、反向标记 |

### 渐变方案

```xml
<linearGradient id="teachingAccent" x1="0%" y1="0%" x2="100%" y2="0%">
  <stop offset="0%" stop-color="#E3660D"/>
  <stop offset="100%" stop-color="#01ABA8"/>
</linearGradient>
```

### 视觉原则

1. 非内容页沿用原模板的校园视觉资产，作为学校品牌锚点
2. 品牌识别集中在页眉与角色化校园图像，不把装饰扩散到内容页主工作区
3. 用橙色做“教学动作”强调，用青绿色做“结构锚点”强调
4. 内容型页面允许较高信息密度，但必须通过留白与轻边框维持阅读节奏

---

## IV. 字体系统

### 字体方案

**Typography direction**: modern CJK teaching sans

| 角色 | 中文 | 英文 | 回退尾部 |
| ---- | ---- | ---- | -------- |
| **Title** | `"Microsoft YaHei", "PingFang SC"` | `Arial` | `sans-serif` |
| **Body** | `"Microsoft YaHei", "PingFang SC"` | `Arial` | `sans-serif` |
| **Emphasis** | `"Microsoft YaHei", "PingFang SC"` | `Arial` | `sans-serif` |
| **Code** | — | `Consolas, "Courier New"` | `monospace` |

**按角色的字体栈**：

- Title: `"Microsoft YaHei", "PingFang SC", Arial, sans-serif`
- Body: `"Microsoft YaHei", "PingFang SC", Arial, sans-serif`
- Emphasis: `"Microsoft YaHei", "PingFang SC", Arial, sans-serif`
- Code: `Consolas, "Courier New", monospace`

### 字号层级

**Baseline**: Body font size = `18px`

| 用途 | 相对 body 的比例 | 当前项目建议 | 字重 |
| ---- | ---------------- | ------------ | ---- |
| 封面标题 | 2.8-3.2x | 50-58px | Bold |
| 章节页标题 | 2.2-2.6x | 40-48px | Bold |
| 页面标题 | 1.5-1.7x | 27-31px | Bold |
| TOC 条目标题 | 1.2-1.4x | 22-25px | Bold |
| 正文内容 | 1x | 18px | Regular |
| 注释 / 辅助文字 | 0.72-0.9x | 13-16px | Regular |
| 页脚 / 页码 | 0.66-0.78x | 12-14px | Regular |

---

## V. 布局原则

### 页面结构

- **Header area**: 左上双色斜切标签 + 顶部分隔线 + 右上角学校横标
- **Content area**: 以宽阔白底主区域承载正文，必要时允许使用轻提示框与结构引导线
- **Footer area**: 细线 + 学校英文全称 / 模板名 / 页码，不额外堆叠信息

### 布局模式库

| 模式 | 适用场景 |
| ---- | -------- |
| **Single column centered** | 封面、结束页、强结论页 |
| **Teaching split (3:7 / 4:6)** | 左侧概念/结论，右侧图示/公式/流程 |
| **Outline list** | 目录页、教学提纲、章节结构总览 |
| **Asymmetric figure-text** | 案例分析、算法框图、实验过程说明 |
| **Matrix / comparison table** | 方法比较、指标权衡、阶段对比 |

### 间距规范

| 元素 | 当前项目 |
| ---- | -------- |
| 距离画布边缘的安全边距 | 72px |
| 内容块之间的主间距 | 24-32px |
| 卡片内边距 | 20-28px |
| 卡片圆角半径 | 16-24px |
| 图标与文字间距 | 8-12px |
| 页内辅助引导线透明度 | 100% 实线或低干扰虚线，不使用复杂装饰纹理 |

### 关键版式判断

1. 封面恢复底部校园全景图，作为课程封面的学校品牌锚点
2. 目录页保留“提纲优先”，左侧列表为主信息，右侧恢复校园建筑照片卡片
3. 内容页保留轻度教学引导：以更轻的左右分区参考和低干扰内容框提示布局，不锁死实际编排
4. 章节页恢复下半部校园剪影，保留章节编号与标题的清晰识别
5. 结束页恢复手绘校园背景，并通过浅色覆层保证文字可读性

---

## VI. 图标使用规范

### 来源

- 默认不在模板中强制预置图标
- 如后续页面需要图标，优先使用 `templates/icons/` 中的线性图标
- 建议选择风格克制的 outline 图标，避免填充型或过度装饰图标破坏教学页面稳定性

### 使用建议

| 用途 | 建议 |
| ---- | ---- |
| 章节导航 | 小尺寸线性图标，可作为目录或分节提示 |
| 方法流程 | 使用统一线宽图标，不超过 2 种颜色 |
| 教学重点标记 | 优先用颜色和编号，不依赖图标堆叠 |

---

## VII. 可视化参考清单

| 可视化类型 | 适用页面 |
| ---------- | -------- |
| 提纲列表 / 教学导航 | 目录页、章节提要页 |
| 对比表 | 方法比较、参数差异、算法选择 |
| 流程图 / 箭头链路 | 设计流程、实验流程、算法框架 |
| 图示 + 注释 | 结构参数说明、公式/图像混排 |
| 双栏图文 | 概念解释、案例问题与解决方案 |

### 参考页映射

- Slide 2：教学提纲页结构
- Slide 10 / 17 / 24：公式/示意/解释型混排
- Slide 31 / 38 / 45：案例与流程框架型内容页
- Slide 52：极简结束页语气

---

## VIII. 图片资源清单

| 文件名 | 用途 | 类型 | 状态 |
| ------ | ---- | ---- | ---- |
| `logo_primary.png` | 右上角学校横标 | Brand asset | Existing |
| `cover_panorama.jpg` | 封面底部校园全景 | Image asset | Existing |
| `campus_building_photo.png` | 目录页右侧校园建筑照片 | Image asset | Existing |
| `chapter_silhouette.png` | 章节页下半部校园剪影 | Image asset | Existing |
| `ending_sketch.jpg` | 结束页手绘校园背景 | Image asset | Existing |

### 资源策略

1. 封面、目录、章节、结束页继承 `sustech` 原始校园视觉资产，保持学校品牌锚点
2. 内容页维持白底教学化增强，不额外引入新的整页背景资产
3. 如需替换或补充图片素材，应回到参考 PPTX 重新提取，并保持当前版式比例与裁切关系

---

## IX. 内容大纲

### Slide 01 - 封面

- **Layout**: 居中标题信息 + 底部校园全景视觉锚点
- **Title**: `{{TITLE}}`
- **Subtitle**: `{{SUBTITLE}}`
- **Info**: `{{AUTHOR}}` / `{{DATE}}`

### Slide 02 - 章节页

- **Layout**: 大号章节编号水印 + 章节标题 + 下半部校园剪影
- **Title**: `{{CHAPTER_TITLE}}`
- **Meta**: `{{CHAPTER_NUM}}`

### Slide 02_toc - 目录页（可选）

- **Layout**: 左侧四项提纲 + 右侧校园建筑照片卡片
- **Content**:
  - `{{TOC_ITEM_1_TITLE}}` / `{{TOC_ITEM_1_DESC}}`
  - `{{TOC_ITEM_2_TITLE}}` / `{{TOC_ITEM_2_DESC}}`
  - `{{TOC_ITEM_3_TITLE}}` / `{{TOC_ITEM_3_DESC}}`
  - `{{TOC_ITEM_4_TITLE}}` / `{{TOC_ITEM_4_DESC}}`

### Slide 03 - 内容页

- **Layout**: 页眉标题 + 主内容框 + 左右分区辅助线 + 教学节奏提示
- **Title**: `{{PAGE_TITLE}}`
- **Content**: `{{CONTENT_AREA}}`
- **Meta**: `{{SOURCE}}` / `{{PAGE_NUM}}`

### Slide 04 - 结束页

- **Layout**: 手绘校园背景 + 浅色覆层 + 中置收束标题 + 联系信息卡片
- **Title**: `{{THANK_YOU}}`
- **Content**: `{{CONTACT_INFO}}`

---

## X. 演讲备注要求

为每一页生成对应的 speaker notes 文件，并保存到 `notes/` 目录：

- **File naming**: 与 SVG 文件名一致，例如 `01_cover.md`
- **封面页**: 讲清课程主题、对象与主线
- **章节页**: 给出本节目标与知识路径
- **内容页**: 强调“问题 → 方法 → 结果/启发”的教学节奏
- **结束页**: 引导提问、讨论或总结收束

---

## XI. 技术约束提醒

### SVG Generation Must Follow

1. viewBox: `0 0 1280 720`
2. 背景使用 `<rect>` 元素
3. 文字换行使用 `<tspan>`，禁止 `<foreignObject>`
4. 透明度使用 `fill-opacity` / `stroke-opacity`；禁止 `rgba()`
5. 禁止：`mask`, `<style>`, `class`, `foreignObject`
6. 禁止：`textPath`, `animate*`, `script`
7. 图片仅允许引用模板目录内真实存在的本地文件
8. 新模板占位符必须采用 canonical contract：
   - Cover: `{{TITLE}}`, `{{SUBTITLE}}`, `{{DATE}}`, `{{AUTHOR}}`
   - Chapter: `{{CHAPTER_NUM}}`, `{{CHAPTER_TITLE}}`
   - Content: `{{PAGE_TITLE}}`, `{{CONTENT_AREA}}`, `{{SOURCE}}`, `{{PAGE_NUM}}`
   - Ending: `{{THANK_YOU}}`, `{{CONTACT_INFO}}`
   - TOC: `{{TOC_ITEM_1_TITLE}}` / `{{TOC_ITEM_1_DESC}}` 等 indexed placeholders

### PPT Compatibility Rules

- 禁止 `<g opacity="...">`
- 不使用 `clipPath`、`mask`、复杂滤镜或外部 CSS
- 所有样式均使用内联属性
- 仅使用安全字体栈：`"Microsoft YaHei", "PingFang SC", Arial, sans-serif` 与 `Consolas, "Courier New", monospace`
