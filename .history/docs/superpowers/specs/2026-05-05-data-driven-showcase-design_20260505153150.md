# Data-Driven Showcase 設計規格

**日期：** 2026-05-05
**狀態：** 待實作

---

## 目標

將 showcase.html 改為資料驅動架構，支援 20+ 個項目，新增項目只需編輯 `projects.js`，無需修改 HTML。

---

## 架構

### 檔案結構

```
claude-website/
├── showcase.html       # 唯一的展示模板（HTML 骨架 + 渲染 JS）
├── projects.js         # 所有項目資料（唯一需要日後編輯的檔案）
└── works.html          # 作品列表（連結更新為 ?id= 格式）
```

### URL 格式

```
showcase.html?id=siemens-tvb
```

---

## 資料結構（projects.js）

```js
const PROJECTS = [
  {
    id: 'siemens-tvb',
    title: 'Siemens × TVB',
    client: 'TVB',
    nature: 'Siemens',
    duration: '30 Seconds Video',
    cover: 'assets/images:/Showcase:/Video:/Project-V01/Cover-V01.webp',
    videoUrl: 'https://www.youtube.com/watch?v=9UlSwBps8-g', // 選填
    services: [
      '2D motion graphic',
      'Graphic and Network',
      'Photos Editing',
      'Online Editing',
      'Colour Grading',
      'Audio Mixing',
      'Video Shooting',
      'Producing',
      'Art Direction',
      'Set Design',
    ]
  }
];
```

**規則：**
- `id` 必填，唯一，用於 URL 參數
- `videoUrl` 選填：有則顯示播放按鈕，無則純封面圖
- `services` 為字串陣列，自動渲染為 tags

---

## showcase.html 改動

### HTML 骨架

現有版面結構不變，在需填資料的元素加上 `data-field` 屬性：

```html
<span data-field="client"></span>
<span data-field="nature"></span>
<span data-field="duration"></span>
<div data-field="cover"></div>
<div data-field="services"></div>
```

### JS 渲染邏輯

1. 讀取 `URLSearchParams` 取得 `?id=`
2. 在 `PROJECTS` 陣列中找對應 object
3. 找不到 → `window.location.href = 'works.html'`（防錯導向）
4. 找到 → 逐欄填入 `data-field` 元素
5. `videoUrl` 存在 → 封面區渲染播放按鈕（點擊開啟 YouTube）；否則純 `<img>`
6. 更新 `document.title` 為 `{title} — Bighead Productions Ltd`

### 引入 projects.js

```html
<script src="projects.js"></script>
<script>
  /* 渲染邏輯 */
</script>
```

---

## works.html 連結格式

每張項目卡片連結更新為：

```html
<a href="showcase.html?id=siemens-tvb">...</a>
```

---

## 不在範圍內

- 年份、類別、獲獎等額外欄位（日後可擴充）
- 項目間的上一個／下一個導航
- 動畫切換效果
