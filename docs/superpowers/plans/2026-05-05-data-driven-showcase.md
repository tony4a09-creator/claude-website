# Data-Driven Showcase Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 將 showcase.html 改為資料驅動架構，新增項目只需編輯 projects.js，不需動 HTML。

**Architecture:** 單一 `showcase.html` 作為模板，讀取 URL `?id=` 參數，在 `projects.js` 找到對應資料後填入 `data-field` 元素。`projects.js` 以 `<script src>` 引入，不需伺服器。

**Tech Stack:** 純 HTML / CSS / Vanilla JS，無框架，無建置工具。

---

## 檔案清單

| 動作 | 路徑 | 職責 |
|------|------|------|
| **新增** | `projects.js` | 所有項目資料陣列（唯一需日後編輯的檔案）|
| **修改** | `showcase.html` | 加入 data-field 屬性 + JS 渲染邏輯 |
| **修改** | `works.html` | 第 425 行連結加上 `?id=siemens-tvb` |

---

## Task 1：建立 projects.js

**Files:**
- Create: `projects.js`

- [ ] **Step 1：建立檔案，寫入 PROJECTS 陣列**

```js
const PROJECTS = [
  {
    id: 'siemens-tvb',
    title: 'Siemens × TVB',
    client: 'TVB',
    nature: 'Siemens',
    duration: '30 Seconds Video',
    cover: 'assets/images:/Showcase:/Video:/Project-V01/Cover-V01.webp',
    videoUrl: 'https://www.youtube.com/watch?v=9UlSwBps8-g',
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
    ],
  },
];
```

- [ ] **Step 2：在瀏覽器 console 驗證**

在瀏覽器開啟任意頁面的 console，輸入：
```js
// 開啟 projects.js 後確認全域變數存在
typeof PROJECTS // 預期："object"
PROJECTS[0].id  // 預期："siemens-tvb"
```

---

## Task 2：修改 showcase.html — HTML 結構

**Files:**
- Modify: `showcase.html`

目標：把靜態內容換成 `data-field` 佔位元素，讓 JS 填入。

- [ ] **Step 1：在 `</head>` 前加入 projects.js 引用**

找到 `showcase.html` 的 `</head>` 標籤，在它**前面**加一行：

```html
  <script src="projects.js"></script>
</head>
```

- [ ] **Step 2：替換封面圖區塊**

找到（約第 367–382 行）：
```html
      <div class="showcase-img-col">
        <div class="showcase-img-wrap reveal">
          <a href="https://www.youtube.com/watch?v=9UlSwBps8-g" target="_blank" rel="noopener noreferrer" class="yt-poster" aria-label="在 YouTube 觀看影片">
            <img
              src="assets/images:/Showcase:/Video:/Project-V01/Cover-V01.webp"
              alt="Siemens × TVB — 30 Seconds Commercial"
              loading="eager"
            >
            <span class="yt-play-btn" aria-hidden="true">
              <svg width="68" height="48" viewBox="0 0 68 48" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="68" height="48" rx="12" fill="rgba(0,0,0,0.72)"/>
                <path d="M28 16l20 8-20 8V16z" fill="white"/>
              </svg>
            </span>
          </a>
        </div>
      </div>
```

替換為：
```html
      <div class="showcase-img-col">
        <div class="showcase-img-wrap reveal" data-field="cover"></div>
      </div>
```

- [ ] **Step 3：替換 meta 欄位靜態值**

找到：
```html
            <div class="meta-item">
              <span class="meta-label">Client:</span>
              <span class="meta-value">TVB</span>
            </div>
            <div class="meta-item">
              <span class="meta-label">Nature:</span>
              <span class="meta-value">Siemens</span>
            </div>
```
以及：
```html
          <div class="meta-item">
            <span class="meta-label">Duration:</span>
            <span class="meta-value">30 Seconds Video</span>
          </div>
```

替換為：
```html
            <div class="meta-item">
              <span class="meta-label">Client:</span>
              <span class="meta-value" data-field="client"></span>
            </div>
            <div class="meta-item">
              <span class="meta-label">Nature:</span>
              <span class="meta-value" data-field="nature"></span>
            </div>
```
以及：
```html
          <div class="meta-item">
            <span class="meta-label">Duration:</span>
            <span class="meta-value" data-field="duration"></span>
          </div>
```

- [ ] **Step 4：替換 services tags-grid**

找到：
```html
          <div class="tags-grid">
            <div class="svc-tag"><span class="tag-dot"></span>2D motion graphic</div>
            <div class="svc-tag"><span class="tag-dot"></span>Graphic and Network</div>
            <div class="svc-tag"><span class="tag-dot"></span>Photos Editing</div>
            <div class="svc-tag"><span class="tag-dot"></span>Online Editing</div>
            <div class="svc-tag"><span class="tag-dot"></span>Colour Grading</div>
            <div class="svc-tag"><span class="tag-dot"></span>Audio Mixing</div>
            <div class="svc-tag"><span class="tag-dot"></span>Video Shooting</div>
            <div class="svc-tag"><span class="tag-dot"></span>Producing</div>
            <div class="svc-tag"><span class="tag-dot"></span>Art Direction</div>
            <div class="svc-tag"><span class="tag-dot"></span>Set Design</div>
          </div>
```

替換為：
```html
          <div class="tags-grid" data-field="services"></div>
```

---

## Task 3：修改 showcase.html — JS 渲染邏輯

**Files:**
- Modify: `showcase.html`（`<script>` 區塊）

- [ ] **Step 1：在現有 `<script>` 區塊頂部加入渲染函式**

找到 `<!-- ══ JS ════ -->` 下方的 `<script>` 開頭，在 `// Nav compact on scroll` **前面**插入以下程式碼：

```js
    // ── Showcase render ──
    (function () {
      const id = new URLSearchParams(window.location.search).get('id');
      const project = (typeof PROJECTS !== 'undefined') && PROJECTS.find(p => p.id === id);
      if (!project) { window.location.href = 'works.html'; return; }

      // Page title
      document.title = project.title + ' — Bighead Productions Ltd';

      // Text fields
      ['client', 'nature', 'duration'].forEach(function (key) {
        var el = document.querySelector('[data-field="' + key + '"]');
        if (el) el.textContent = project[key] || '';
      });

      // Cover / video
      var coverEl = document.querySelector('[data-field="cover"]');
      if (coverEl) {
        if (project.videoUrl) {
          coverEl.innerHTML =
            '<a href="' + project.videoUrl + '" target="_blank" rel="noopener noreferrer" class="yt-poster" aria-label="在 YouTube 觀看影片">' +
              '<img src="' + project.cover + '" alt="' + project.title + '" loading="eager">' +
              '<span class="yt-play-btn" aria-hidden="true">' +
                '<svg width="68" height="48" viewBox="0 0 68 48" fill="none" xmlns="http://www.w3.org/2000/svg">' +
                  '<rect width="68" height="48" rx="12" fill="rgba(0,0,0,0.72)"/>' +
                  '<path d="M28 16l20 8-20 8V16z" fill="white"/>' +
                '</svg>' +
              '</span>' +
            '</a>';
        } else {
          coverEl.innerHTML =
            '<img src="' + project.cover + '" alt="' + project.title + '" loading="eager">';
        }
      }

      // Services tags
      var servicesEl = document.querySelector('[data-field="services"]');
      if (servicesEl && project.services) {
        servicesEl.innerHTML = project.services.map(function (s) {
          return '<div class="svc-tag"><span class="tag-dot"></span>' + s + '</div>';
        }).join('');
      }
    })();
```

- [ ] **Step 2：瀏覽器驗證 — 正常情況**

在瀏覽器開啟：
```
showcase.html?id=siemens-tvb
```

預期：
- 封面圖顯示，點擊播放鍵開啟 YouTube
- Client 顯示「TVB」、Nature 顯示「Siemens」、Duration 顯示「30 Seconds Video」
- Services 顯示 10 個 tag
- 瀏覽器分頁標題為「Siemens × TVB — Bighead Productions Ltd」

- [ ] **Step 3：瀏覽器驗證 — 錯誤 id**

在瀏覽器開啟：
```
showcase.html?id=does-not-exist
```

預期：自動跳轉回 `works.html`

- [ ] **Step 4：瀏覽器驗證 — 無 id**

在瀏覽器開啟：
```
showcase.html
```

預期：自動跳轉回 `works.html`

---

## Task 4：更新 works.html 連結

**Files:**
- Modify: `works.html`（約第 425 行）

- [ ] **Step 1：更新 Siemens TVB 卡片連結**

找到（約第 425 行）：
```html
        <a href="showcase.html" class="work-card" data-cat="video">
```

替換為：
```html
        <a href="showcase.html?id=siemens-tvb" class="work-card" data-cat="video">
```

- [ ] **Step 2：瀏覽器驗證**

開啟 `works.html`，點擊 Siemens TVB 卡片，確認正確跳轉至 `showcase.html?id=siemens-tvb` 並顯示完整內容。
