# Executor Common Guidelines

> Style-specific content is in the corresponding `executor-{style}.md`. Technical constraints are in shared-standards.md.

---

## 1. Template Adherence Rules

If template files exist in the project's `templates/` directory, the template structure must be followed:

| Page Type | Corresponding Template | Adherence Rules |
|-----------|----------------------|-----------------|
| Cover | `01_cover.svg` | Inherit background, decorative elements, layout structure; replace placeholder content |
| Chapter | `02_chapter.svg` | Inherit numbering style, title position, decorative elements |
| Content | `03_content.svg` | Inherit header/footer styles; **content area may be freely laid out** |
| Ending | `04_ending.svg` | Inherit background, thank-you message position, contact info layout |
| TOC | `02_toc.svg` | **Optional**: Inherit TOC title, list styles |

### Page-Template Mapping Declaration (Required Output)

Before generating each page, you must explicitly output which template (or "free design") is used:

```
📝 **Template mapping**: `templates/01_cover.svg` (or "None (free design)")
🎯 **Adherence rules / layout strategy**: [specific description]
```

- **Content pages**: Templates only define header and footer; the content area is freely laid out by the Executor
- **No template**: Generate entirely per the Design Specification & Content Outline

---

## 2. Design Parameter Confirmation (Mandatory Step)

> Before generating the first SVG page, you **must review the key design parameters from the Design Specification & Content Outline** to ensure all subsequent generation strictly follows the spec.

Must output confirmation including: canvas dimensions, body font size, color scheme (primary/secondary/accent HEX values), font plan.

**Why is this step mandatory?** Prevents the "spec says one thing, execution does another" disconnect.

### 2.1 Per-page spec_lock re-read (Mandatory)

> Long decks + streaming generation = attention drift and context compression. By mid-deck, the Executor has often drifted off the color palette or icon inventory declared in `design_spec.md`. `spec_lock.md` is the short form of those decisions and is the Executor's **canonical execution reference** during generation.

**Hard rule**: Before generating **each** SVG page, `read_file <project_path>/spec_lock.md`. Use the values from this file — not values you remember from earlier in the conversation. If the context was auto-compacted, `read_file <project_path>/design_spec.md` too (for the current page's §IX brief).

**If `spec_lock.md` is missing**: Before generating each page, emit the literal line `warning: spec_lock.md missing — generating without execution lock` and proceed using `design_spec.md` narrative values. Do not silently skip. A missing lock is expected only for legacy projects predating this feature; for new projects the Strategist MUST produce it (see [strategist.md](strategist.md) §6, step 4).

**Forbidden — values outside the lock**:

- Color values (fill / stroke / stop-color) MUST come from `colors` in `spec_lock.md`
- Icons MUST come from the `icons.inventory` list; icon library MUST equal `icons.library`
- Font family MUST come from the `typography` block: use the role-specific override (`title_family` / `body_family` / `emphasis_family` / `code_family`) if declared for that role, otherwise fall back to `font_family`.
- Font sizes follow a **ramp anchored on `typography.body`**, not a closed menu. The slots listed in `spec_lock.md typography` (`title` / `subtitle` / `annotation` / any project-specific additions like `cover_title` / `hero_number`) are common anchors — use them directly when they fit. Executor MAY use an intermediate size (e.g., 40px hero number, 13px chart annotation, 72px cover headline) when the role calls for it, **provided** the size's ratio to `body` falls within the corresponding role's band in the design-spec ramp table (see `design_spec_reference.md §IV — Font Size Hierarchy`; for Chinese workflows, prefer `design_spec_reference_CN.md §IV`; also mirrored in `design_spec.md §IV` per project). Sizes outside every ramp band are still forbidden — surface the need and extend the lock instead of inventing one-off values.
- Images MUST reference files listed under `images`; no invented filenames

If a page genuinely needs a value not in `spec_lock.md`, stop and surface it — do not silently invent one. Either request the user to extend the lock, or revise the page to stay within it.

**Per-page layout rhythm — `page_rhythm` section**:

Before drawing each page, look up its entry in `page_rhythm` (key format `P<NN>` matching the page index in §IX of `design_spec.md`) and apply the corresponding layout discipline:

| Tag | Layout discipline |
|-----|-------------------|
| `anchor` | Structural page (cover / chapter / TOC / ending). Follow the matching template verbatim. |
| `dense` | Information-heavy. Card grids, multi-column layouts, KPI dashboards, tables, and charts are all permitted. This is the baseline behavior. |
| `breathing` | Low-density impact page. Avoid **multi-card grid layouts** — do not organize content as multiple parallel rounded containers (3-card row, 4-card KPI grid, 2×2 matrix rendered as cards). Use naked text blocks, dividers, whitespace, or full-bleed imagery as the content structure. Single rounded visual elements (hero image corners, callouts, tags, one emphasis block) are fine — the rule is about grid structure, not about the `rx` attribute. Proportions follow information weight (not a preset ratio). Typical forms: hero quote, single large number with one-line interpretation, full-bleed image with floating caption, section transition. |

**Why this matters**: Without rhythm variation, long decks default to "every page is a card grid" — the AI-generated look. The `page_rhythm` tag is the only lever that survives context compression (since Executor re-reads `spec_lock.md` per page); narrative guidance buried in `design_spec.md` does not.

**Missing `page_rhythm` section** → fall back to `dense` for every page (pre-rhythm behavior). Emit the literal line `warning: spec_lock.md missing page_rhythm — defaulting all pages to dense` once, then proceed.

**Tag not found for current page** → fall back to `dense` silently. Do not invent a tag.

**Rationale**: Tool-result re-reads bypass model memory (which compression can corrupt). Every page gets a fresh ground truth pinned to the most recent turn in context.

---

## 3. Execution Guidelines

- **Proximity principle**: Place related elements close together to form visual groups; increase spacing between unrelated groups to reinforce logical structure
- **Absolute spec adherence**: Strictly follow the color, layout, canvas format, and typography parameters in the spec
- **Follow template structure**: If templates exist, inherit the template's visual framework
- **Main-agent ownership**: SVG generation must be performed by the current main agent, not delegated to sub-agents, because each page depends on shared upstream context and cross-page visual continuity
- **Generation rhythm**: First lock the global design context, then generate pages sequentially one by one in the same continuous context; grouped page batches (for example, 5 pages at a time) are not allowed
- **Phased batch generation** (recommended):
  1. **Visual Construction Phase**: Generate all SVG pages continuously in sequential page order, ensuring high consistency in design style and layout coordinates (Visual Consistency)
  2. **Quality Check Gate** (mandatory between phases): run `python3 scripts/svg_quality_checker.py <project_path>` against `svg_output/`. Any `error` (banned SVG features, viewBox mismatch, spec_lock color / font / size drift, non-PPT-safe font stack, etc.) MUST be fixed on the offending page before entering the Logic Construction Phase — re-generate that page and re-run the check. `warning` entries should be reviewed and fixed when straightforward; otherwise acknowledge and release. Do NOT defer this check to after `finalize_svg.py` — finalize rewrites SVG and some violations get masked.
  3. **Logic Construction Phase**: After SVGs pass the quality gate, batch-generate speaker notes to ensure narrative coherence (Narrative Continuity)
- **Technical specifications**: See [shared-standards.md](shared-standards.md) for SVG technical constraints and PPT compatibility rules
- **Visual depth**: Use filter shadows, glow effects, gradient fills, dashed strokes, and gradient overlays from shared-standards.md to create layered depth — flat pages without elevation or emphasis look unfinished

### SVG File Naming Convention

File naming format: `<number>_<page_name>.svg`

- **Chinese content** → Chinese naming: `01_封面.svg`, `02_目录.svg`, `03_核心优势.svg`
- **English content** → English naming: `01_cover.svg`, `02_agenda.svg`, `03_key_benefits.svg`
- **Number rules**: Two-digit numbers, starting from 01
- **Page name**: Concise and descriptive, matching the page title in the Design Specification & Content Outline

---

## 4. Icon Usage

Four approaches: **A: Emoji** (`<text>🚀</text>`) | **B: AI-generated** (SVG basic shapes) | **C: Built-in library** (`templates/icons/` 6700+ icons, recommended) | **D: Custom** (user-specified)

**Built-in icons — Placeholder method (recommended)**:

```xml
<!-- chunk (default — straight-line geometry, sharp corners, structured) -->
<use data-icon="chunk/home" x="100" y="200" width="48" height="48" fill="#005587"/>

<!-- tabler-filled (bezier-curve forms, smooth & rounded contours) -->
<use data-icon="tabler-filled/home" x="100" y="200" width="48" height="48" fill="#005587"/>

<!-- tabler-outline (light, line-art style — screen-only decks) -->
<use data-icon="tabler-outline/home" x="100" y="200" width="48" height="48" fill="#005587"/>
```

> No need to manually run `embed_icons.py`; `finalize_svg.py` post-processing tool will auto-embed icons.

**Three icon libraries**:

| Library | Style | Count | Prefix | When to use |
|---------|-------|-------|--------|-------------|
| `chunk` | fill · straight-line geometry (sharp corners, rectilinear) | 640 | `chunk/` | ✅ **Default** — most scenarios |
| `tabler-filled` | fill · bezier-curve forms (smooth, rounded contours) | 1000+ | `tabler-filled/` | When design calls for smooth, rounded, organic icon forms |
| `tabler-outline` | stroke/line | 5000+ | `tabler-outline/` | Screen-only decks needing a light, elegant aesthetic |

> ⚠️ **One presentation = one library.** Never mix icons from different libraries. If the chosen library lacks an exact icon, find the closest available alternative **within that same library** — do not cross into another library to fill the gap.

**Searching for icons** — use terminal, zero token cost:
```bash
ls skills/ppt-master/templates/icons/chunk/ | grep home
ls skills/ppt-master/templates/icons/tabler-filled/ | grep home
ls skills/ppt-master/templates/icons/tabler-outline/ | grep chart
```

**Abstract concept → icon name** (names for `chunk`; tabler libraries use their own equivalents — verify with `ls | grep`):

| Concept | chunk | tabler-filled / tabler-outline |
|---------|-------|-------------------------------|
| Growth / Increase | `arrow-trend-up` | same |
| Decline / Decrease | `arrow-trend-down` | same |
| Success / Complete | `circle-checkmark` | `circle-check` |
| Warning / Risk | `triangle-exclamation` | `alert-triangle` |
| Innovation / Idea | `lightbulb` | `bulb` |
| Strategy / Goal | `target` | same |
| Efficiency / Speed | `bolt` | same |
| Collaboration / Team | `users` | same |
| Settings / Config | `cog` | `settings` |
| Security / Trust | `shield` | same |
| Money / Finance | `dollar` | `currency-dollar` |
| Time / Deadline | `clock` | same |
| Location / Region | `map-pin` | same |
| Communication | `comment` | `message` |
| Analysis / Data | `chart-bar` | same |
| Process / Flow | `arrows-rotate-clockwise` | `refresh` |
| Global / World | `globe` | `world` |
| Excellence / Award | `star` | same |
| Expand / Scale | `maximize` | same |
| Problem / Issue | `bug` | same |

> For self-evident names (home, user, file, search, arrow, etc.) — just `grep chunk/` directly without consulting the table.

> ⚠️ **Icon validation rule**: If the Design Specification includes an icon inventory list, Executor may **only** use icons from that approved list. Before using any icon, verify it exists via `ls | grep` search. **Mixing icons from different libraries in the same presentation is FORBIDDEN** — use only the library specified in the Design Spec.

---

## 5. Visualization Reference

When the Design Spec includes a **VII. Visualization Reference List**, read the referenced SVG templates from `templates/charts/` before drawing pages that use those visualization types. The path remains `templates/charts/` for backward compatibility.

🚧 **GATE — Mandatory read before first use**: When Executor encounters a visualization type listed in Section VII of the Design Spec for the first time, Executor **MUST** `read_file templates/charts/<chart_name>.svg` **before** generating that page. Extract the layout coordinates, card structure, spacing rhythm, and visual logic from the template as **creative reference and inspiration** — not as a strict copy. Then design the page independently using the project's own color scheme, typography, and content.

> **Workflow**: read template SVG → understand structure & spacing → design original SVG informed by the reference → do NOT replicate the template verbatim.
> **Reuse**: Once a visualization type has been read and understood, there is no need to re-read for subsequent pages of the same type.
> **Change**: Read the new template when the visualization type changes or the structure needs re-reference.

**Adaptation rules**:
- **Must preserve**: Visualization type (bar/line/pie/timeline/process/framework etc.) as specified in the Design Spec
- **Must adapt**: Data values, labels, colors (match the project's color scheme), and dimensions to fit the page layout
- **May adjust freely**: Visual composition, axis ranges, grid lines, legend position, spacing, decorative elements — creative freedom is encouraged as long as the chart remains accurate and readable
- **Must NOT**: Change visualization type without Design Spec justification, or omit data points / structural elements specified in the outline

> Visualization templates: `templates/charts/` (57 types). Index: `templates/charts/charts_index.json`

---

## 6. Image Handling

Handle images based on their status in the Design Specification's "Image Resource List":

| Status | Source | Handling |
|--------|--------|----------|
| **Existing** | User-provided | Reference images directly from `../images/` directory |
| **AI-generated** | Generated by Image_Generator | Images already in `../images/`, reference directly |
| **Placeholder** | Not yet prepared | Use dashed border placeholder |

**Reference**: `<image href="../images/xxx.png" ... preserveAspectRatio="xMidYMid slice"/>`

**Placeholder**: Dashed border `<rect stroke-dasharray="8,4" .../>` + description text

---

## 7. Font Usage

Font family per role is governed by `spec_lock.md typography` — that file is the source of truth. Read `font_family` as the default, plus any `*_family` override (`title_family` / `body_family` / `emphasis_family` / `code_family` / etc.); roles without an explicit override fall back to `font_family`.

If `spec_lock.md` is absent for any reason, consult the seed combinations and PPT-safe discipline in [`strategist.md`](strategist.md) §g "Typography Plan Confirmation" rather than inventing a stack.

**Hard rule — never violate**: every `font-family` stack emitted into SVG MUST end with a cross-platform pre-installed family (Microsoft YaHei / SimHei / SimSun / Arial / Calibri / Segoe UI / Times New Roman / Georgia / Consolas / Courier New / Impact / Arial Black). PPTX stores a single `typeface` per run with no runtime fallback — a missing font silently degrades to Calibri on the viewer's machine.

---

## 8. Speaker Notes Generation Framework

### Task 1. Generate Complete Speaker Notes Document

After **all SVG pages are generated and finalized**, enter the "Logic Construction Phase" and generate the complete speaker notes document in `notes/total.md`.

**Why not generate page-by-page?** Batch-writing notes allows planning transitions like a script, ensuring coherent presentation logic.

**Format**: Each page starts with `# <number>_<page_title>`, separated by `---` between pages. Each page includes: script text (2-5 sentences), `Key points: ① ② ③`, `Duration: X minutes`. Except for the first page, each page's text starts with a `[Transition]` phrase.

**Basic stage direction markers** (common to all styles):

| Marker | Purpose |
|--------|---------|
| `[Pause]` | Whitespace after key content, letting the audience absorb |
| `[Transition]` | Standalone paragraph at the start of each page's text, bridging from the previous page |

> Each style may extend with additional markers (`[Interactive]`/`[Data]`/`[Scan Room]`/`[Benchmark]` etc.), see `executor-{style}.md`.

**Language consistency rule**: All structural labels and stage direction markers in speaker notes **MUST match the presentation's content language**. When the presentation content is non-English, localize every label — do NOT mix English labels with non-English content.

| English | 中文 | 日本語 | 한국어 |
|---------|------|--------|--------|
| `[Transition]` | `[过渡]` | `[つなぎ]` | `[전환]` |
| `[Pause]` | `[停顿]` | `[間]` | `[멈춤]` |
| `[Interactive]` | `[互动]` | `[問いかけ]` | `[상호작용]` |
| `[Data]` | `[数据]` | `[データ]` | `[데이터]` |
| `[Scan Room]` | `[观察]` | `[観察]` | `[관찰]` |
| `[Benchmark]` | `[对标]` | `[ベンチマーク]` | `[벤치마크]` |
| `Key points:` | `要点：` | `要点：` | `핵심 포인트:` |
| `Duration:` | `时长：` | `所要時間：` | `소요 시간:` |
| `Flex:` | `弹性：` | `調整：` | `조정:` |

> For languages not listed above, translate each label to the corresponding natural term in that language.

**Requirements**:

- Notes should be conversational and flow naturally
- Highlight each page's core information and presentation key points
- Users can manually edit and override in the `notes/` directory

### Task 2. Split Into Per-Page Note Files

Automatically split `notes/total.md` into individual speaker note files in the `notes/` directory.

**File naming convention**:

- **Recommended**: Match SVG names (e.g., `01_cover.svg` → `notes/01_cover.md`)
- **Compatible**: Also supports `slide01.md` format (backward compatibility)

---

## 9. Next Steps After Completion

> **Auto-continuation**: After Visual Construction Phase (all SVG pages) and Logic Construction Phase (all notes) are complete, the Executor proceeds directly to the post-processing pipeline.

**Post-processing & Export** (see [shared-standards.md](shared-standards.md)):

```bash
# 1. Split speaker notes
python3 scripts/total_md_split.py <project_path>

# 2. SVG post-processing (auto-embed icons, images, etc.)
python3 scripts/finalize_svg.py <project_path>

# 3. Export PPTX
python3 scripts/svg_to_pptx.py <project_path> -s final
# Output: exports/<project_name>_<timestamp>.pptx + exports/<project_name>_<timestamp>_svg.pptx
```
