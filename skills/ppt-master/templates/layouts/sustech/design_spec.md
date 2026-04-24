# SUSTech Brand Template - Design Specification

> A light academic template reconstructed from the official SUSTech reference PPT, suitable for research presentations, academic defenses, campus briefings, and technology-oriented university reports.

---

## I. Template Overview

| Property | Description |
| --- | --- |
| **Template Name** | SUSTech Brand Template |
| **Template ID** | `sustech` |
| **Category** | Brand |
| **Use Cases** | Academic defense, research report, course presentation, university introduction, campus technology briefing |
| **Design Tone** | Modern academic · international university brand · light technology texture |
| **Reference Basis** | Rebuilt from `examples/SUSTech PPT template.pptx` using extracted assets, theme metadata, slide-layout XML, and slide content XML |

### Brand Reconstruction Notes

1. **Header DNA from `slideLayout2.xml`**: retain the small orange / teal skewed tabs in the upper-left corner, the top hairline, the right-aligned school logo, and the restrained footer line.
2. **Campus identity from extracted assets**: use the waterfront panorama on the cover, the campus photo on TOC, the orange silhouette on chapter pages, and the sketch illustration on the ending page.
3. **Palette grounding**: combine the imported layout accent colors `#01ABA8` and `#E3660D` with the dark teal extracted from the school wordmark.
4. **Structure decision**: the original PPT reference contains a cover, repeated content layouts, a silhouette interstitial, and an ending slide; this library template extends that system into a reusable 5-page package.

---

## II. Canvas Specification

| Property | Value |
| --- | --- |
| **Format** | Standard 16:9 |
| **Dimensions** | 1280 × 720 px |
| **viewBox** | `0 0 1280 720` |
| **Header Rule** | `y = 76` |
| **Footer Rule** | `y = 688` |
| **Content Safe Area** | `x: 72–1208`, `y: 118–628` |
| **Recommended Outer Margins** | 72 px left/right, 44 px top, 32 px bottom |

---

## III. Color Scheme

### Core Brand Colors

| Role | Value | Source |
| --- | --- | --- |
| **SUSTech Deep Teal** | `#0F373B` | Extracted from the horizontal school wordmark |
| **SUSTech Orange** | `#E3660D` | Extracted from the original PPT layout accent |
| **SUSTech Bright Teal** | `#01ABA8` | Extracted from the original PPT layout accent |
| **Warm White** | `#FFFFFF` | Original PPT page background |
| **Paper Gray** | `#F7F6F2` | Supporting neutral for image framing |
| **Rule Gray** | `#D9D9D9` | Top/bottom divider lines |

### Text Colors

| Role | Value | Usage |
| --- | --- | --- |
| **Primary Title** | `#0F373B` | Cover title, chapter title, ending title |
| **Body Text** | `#3A3838` | Main explanatory text |
| **Secondary Text** | `#6F6F6F` | Subtitle, descriptions, footer metadata |
| **Muted Label** | `#9D9D9D` | Footer brand line, guidance text |

### Recommended Ratio

- Deep teal: 50%
- White / warm neutrals: 35%
- Orange accents: 10%
- Bright teal accents: 5%

---

## IV. Typography System

### Font Family

Imported theme fonts show `majorLatin=微软雅黑`, `minorLatin=微软雅黑`, `minorEastAsia=微软雅黑`.

**Recommended stack**:

`"Microsoft YaHei", "微软雅黑", "PingFang SC", Arial, sans-serif`

### Type Scale

| Level | Usage | Size | Weight |
| --- | --- | --- | --- |
| **H1** | Cover title | 48 px | Bold |
| **H2** | Ending title / chapter title | 42–52 px | Bold |
| **H3** | Content / TOC title | 28–32 px | Bold |
| **H4** | TOC item title | 22–24 px | Bold |
| **Body** | Normal body text | 16–18 px | Regular |
| **Meta** | Footer / captions | 12–14 px | Regular |
| **Display** | Chapter number watermark | 150–170 px | Bold |

---

## V. Page Structure

### Shared Visual Language

1. **Upper-left dual skewed tabs**  
   Two small parallelogram accents partially extend outside the canvas. The front tab uses orange, the rear tab uses bright teal.

2. **Right-aligned logo band**  
   The top-right horizontal logo sits within the top rule zone and should stay visually light without blocking content.

3. **Hairline framing**  
   A thin top divider and bottom divider preserve the quiet academic structure of the original PPT.

4. **Campus imagery by role**  
   - Cover: panoramic waterfront skyline  
   - TOC: large campus building photo card  
   - Chapter: orange silhouette band  
   - Ending: full-canvas hand-drawn campus sketch

---

## VI. Page Types

### 1. Cover Page (`01_cover.svg`)

- White academic background
- Upper-left dual skewed tabs
- Right-aligned school logo
- Centered title block
- Bottom panoramic campus image as the signature visual anchor

### 2. Chapter Page (`02_chapter.svg`)

- White background with a giant low-opacity chapter number
- Left-aligned chapter title
- Orange silhouette image spanning the lower third
- Brand footer and top-right logo retained

### 3. Table of Contents (`02_toc.svg`)

- Same header / footer system as content pages
- Four numbered TOC items on the left
- Campus photo framed as a right-side supporting visual

### 4. Content Page (`03_content.svg`)

- Directly inherits the original PPT layout grammar
- Title in the upper-left
- Flexible central content area with a light guidance frame
- Footer supports `{{SOURCE}}` and `{{PAGE_NUM}}`

### 5. Ending Page (`04_ending.svg`)

- Full-canvas sketch illustration from the extracted asset
- Light white overlay for text legibility
- Centered thank-you message
- Structured contact box

---

## VII. Layout Modes (Recommended)

| Mode | Recommended Usage | Notes |
| --- | --- | --- |
| **Anchor** | Cover, chapter, ending | Use strong hero title and keep surrounding content sparse |
| **Breathing** | TOC, summary slides, high-level overviews | Leave generous whitespace around list modules |
| **Dense** | Data explanation or methodology pages | Use the content template and fill the central safe area with charts / cards / diagrams |

---

## VIII. Spacing Specification

| Element | Value |
| --- | --- |
| Header left inset | 72 px |
| Header logo area | `x = 1018–1268`, `y = 0–76` |
| Title left inset (content pages) | 72 px |
| Primary content frame | `x = 72`, `y = 118`, `w = 1136`, `h = 510` |
| Footer text baseline | 706 px |
| TOC item vertical interval | 102 px |
| Chapter silhouette top edge | 388 px |

---

## IX. SVG Technical Constraints

1. All SVG files must keep `viewBox="0 0 1280 720"`.
2. Do not use `<style>`, `class`, `mask`, `foreignObject`, or `rgba()`.
3. Transparency must use `fill-opacity` or `stroke-opacity`.
4. Images must be referenced by local asset filenames inside the template directory.
5. Content-page guides must remain lightweight and non-blocking for downstream AI slide generation.

---

## X. Placeholder Specification

### Cover

| Placeholder | Purpose |
| --- | --- |
| `{{TITLE}}` | Main title |
| `{{SUBTITLE}}` | Subtitle |
| `{{DATE}}` | Date |
| `{{AUTHOR}}` | Presenter / organization |

### Chapter

| Placeholder | Purpose |
| --- | --- |
| `{{CHAPTER_NUM}}` | Chapter number |
| `{{CHAPTER_TITLE}}` | Chapter title |

### TOC

| Placeholder | Purpose |
| --- | --- |
| `{{TOC_ITEM_1_TITLE}}` – `{{TOC_ITEM_4_TITLE}}` | TOC item titles |
| `{{TOC_ITEM_1_DESC}}` – `{{TOC_ITEM_4_DESC}}` | Optional TOC item descriptions |

### Content

| Placeholder | Purpose |
| --- | --- |
| `{{PAGE_TITLE}}` | Page title |
| `{{CONTENT_AREA}}` | Flexible content area marker |
| `{{SOURCE}}` | Source / note text |
| `{{PAGE_NUM}}` | Page number |

### Ending

| Placeholder | Purpose |
| --- | --- |
| `{{THANK_YOU}}` | Thank-you message |
| `{{CONTACT_INFO}}` | Contact information |

---

## XI. Usage Guide

1. Best for university, research, and campus-tech narratives that need a clean institutional identity instead of a heavy corporate style.
2. Keep page backgrounds light; the template already contains enough brand features through imagery and header accents.
3. Prefer dark-teal titles and orange emphasis numbers; avoid introducing unrelated saturated colors.
4. When slides become data-heavy, keep the content inside the safe zone and preserve the top / bottom framing lines.
