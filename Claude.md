# Bighead Productions Ltd — Website Project Guidelines

## 專案概覽 Project Overview

單一頁面 Landing Page，服務於 Bighead Productions Ltd（香港創意製作社群）。
所有 HTML、CSS、JS 均內嵌於單一檔案：`index.html`。無框架，純原生 Web。

---

## 品牌規範 Brand Guidelines

### 字體 Typography
- **主字體**: DM Sans（Google Fonts）
- 粗細使用：300（italic 用於副標）、400、600

### 調色板 Colour Palette
| 名稱 | 值 |
|---|---|
| `--black` | `#111110` |
| `--dark` | `#1c1c1b` |
| `--mid` | `#3c3c3c`（品牌 SVG 預設填色）|
| `--light` | `#f5f3ef` |
| `--champagne` | `#d4c9b0` |
| `--white` | `#ffffff` |
| `--muted` | `#888` |

### Logo
- 檔案：`assets/BH/brand/bighead-logo.svg`
- ViewBox：`175.27 × 35.96`，fill `#3c3c3c`
- 深色背景時用 `filter: brightness(0) invert(1)` 轉白

---

## 資源路徑 Asset Paths

| 資源 | 路徑 |
|---|---|
| 品牌 Logo（深色用）| `assets/BH/brand/bighead-logo.svg` |
| 品牌 Logo（淺色用）| `assets/BH/icons/Light_Logo.svg` |
| Instagram icon | `assets/BH/icons/icon_IG.svg` |
| Vimeo icon | `assets/BH/icons/icon_Viemo.svg` |
| Facebook icon | `assets/BH/icons/icon_FB.svg` |
| 品牌規範圖 | `assets/BH/brand/BH_Brand_Guidelines_01.png` |

---

## 頁面結構 Page Structure

```
Nav → Hero (Think BIG split) → Logo Strip → About → Services → Projects (dark bg) → CTA → Footer
```

### 各區段重點
- **Hero**: CSS Grid `1fr 1fr`；左文字、右照片拼貼（4 張）；`min-height: 100svh`
- **Services**: 卡片 hover → 背景變黑、文字變白
- **Projects**: 深色背景 (`--dark`)，5 張專案卡
- **Footer**: 深色背景，三欄 Grid，底部含社群 icon 圓形按鈕

---

## 社群 Icon 規範 Social Icons

品牌 icon SVG（IG、Vimeo、FB）的 `viewBox="0 0 45 45"`，路徑包含外框圓角方塊 + 圖示，fill 固定為 `#3c3c3c`。

### 正確做法：使用 `<img>` 標籤
```html
<a href="#" class="soc-btn" aria-label="Instagram">
  <img src="assets/BH/icons/icon_IG.svg" alt="" width="20" height="20">
</a>
```

### CSS 顏色控制（filter）
```css
.soc-btn img {
  width: 20px; height: 20px;
  display: block;
  filter: brightness(0) invert(1);   /* 深色背景 → 白色 */
  opacity: 0.45;
  transition: filter 0.3s, opacity 0.3s;
}
.soc-btn:hover img {
  filter: brightness(0);             /* hover → 黑色 */
  opacity: 1;
}
```

### 為何不用 inline SVG
- 品牌 SVG 含 `<defs>` + `clip-path: url(#clippath)` 參照，inline 後 ID 衝突易導致渲染問題
- `<img>` + CSS filter 是最穩定的顏色控制方案

### 圓形按鈕置中
`.soc-btn` 使用 `display: flex; align-items: center; justify-content: center`，20px img 自動置中於 34px 圓形容器。

---

## 資產資料夾結構 Asset Folder Structure

品牌優先（Brand-first）結構，每個品牌為頂層分類：

```
assets/
├── BH/                  ← 主品牌 Bighead Productions
│   ├── brand/           ← Logo SVG、品牌規範 PNG
│   ├── icons/           ← 所有 UI icon SVG（含 Company_Structure/ 子資料夾）
│   └── images/
│       ├── Client_logo/ ← 客戶 logo（V1/V2/V3 等版本）
│       ├── Company_structrue/
│       ├── HeroVideo/   ← 首頁 hero 影片
│       ├── Showcase/
│       │   ├── Video/   ← 影片專案封面（Project-V01 … V013）
│       │   └── Event/   ← 活動專案照片（Project-E01 … E03）
│       └── thumbnails/
└── Biglab/              ← 子品牌 Biglab（骨架備用）
    ├── brand/
    ├── icons/
    └── images/
        ├── HeroVideo/
        ├── Showcase/
        │   ├── Video/
        │   └── Event/
        └── thumbnails/
```

## 已知技術決策 Technical Decisions

| 決策 | 原因 |
|---|---|
| 不使用 inline SVG 作為 icon | clip-path 內部 ID 衝突 |
| 圖片暫用 CSS gradient placeholder | placehold.co 替換尚未完成（使用者中途取消） |
| `filter: brightness(0) invert(1)` | 控制深色 SVG 在深色背景顯示為白色 |
| `100svh` 代替 `100vh` | 行動裝置瀏覽器工具列修正 |
| 資料夾不用冒號命名 | 原 `icon:`、`images:` 等冒號名稱已於品牌重組時清除 |

---

## 待完成項目 Pending Tasks

- [ ] 將所有 CSS gradient 佔位圖替換為 `https://placehold.co/` URL：
  - Hero 照片拼貼 × 4（`.hero-photo:nth-child(n) .hero-photo-fill`）
  - About 圖片 × 1（`.about-img-fill`）
  - Project 卡片 × 5（`.proj-card:nth-child(n) .proj-bg`）
- [ ] 替換為真實照片後移除 placehold.co

---

## 語言偏好 Language Preference

使用者以中文（繁體）或中英夾雜溝通，技術術語保留英文。回覆可中英混用。
