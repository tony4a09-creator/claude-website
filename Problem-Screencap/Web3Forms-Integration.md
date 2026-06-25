# Web3Forms 聯絡表單整合指南

**目標：** 讓 `contact.html` 的聯絡表單真正發送電郵到 `sales@bighead.com.hk`
**方案：** Web3Forms — 免費、無限次提交、無需後端

---

## 為什麼選 Web3Forms

| | Web3Forms | Formspree Free | EmailJS Free |
|---|---|---|---|
| 費用 | 免費 | 免費 | 免費 |
| 每月上限 | **無限** | 50 次 | 200 次 |
| 設定難度 | 簡單 | 簡單 | 中等 |
| Access Key 安全性 | Domain 白名單保護 | 同樣機制 | 同樣機制 |

---

## 安全說明

Access Key 會出現在 HTML 原始碼（hidden input），任何人都能看到。
**最大風險：** 有人拿 key 向 `sales@bighead.com.hk` 發送垃圾郵件。
**防護方法：** 在 Web3Forms dashboard 設定 Domain 白名單，只接受來自 `bighead.com.hk` 的提交。

---

## Step 1 — 申請 Access Key

1. 前往 **https://web3forms.com**
2. 在畫面中央輸入框輸入 `sales@bighead.com.hk`
3. 按 **"Get Access Key"**
4. 開啟 `sales@bighead.com.hk` 收件匣，找到 Web3Forms 確認信
5. 複製 Access Key（格式：`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`）

---

## Step 2 — 修改 contact.html

### 2a. 在 `<form id="contactForm">` 內加入 hidden input

在 `<form class="contact-form reveal d1" id="contactForm" novalidate>` 開頭的下一行加：

```html
<input type="hidden" name="access_key" value="貼上你的Access Key">
```

### 2b. 替換 JS 表單提交邏輯

找到 `contact.html` 底部的 `// Form submit` 區段（約第 899 行），將整個 `form.addEventListener` 區塊替換為：

```js
// Form submit
const form = document.getElementById('contactForm');
const success = document.getElementById('formSuccess');
form.addEventListener('submit', async e => {
  e.preventDefault();
  if (!form.checkValidity()) { form.reportValidity(); return; }

  const btn = form.querySelector('.form-submit');
  btn.disabled = true;

  const data = Object.fromEntries(new FormData(form));
  const res = await fetch('https://api.web3forms.com/submit', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
    body: JSON.stringify(data)
  });

  if (res.ok) {
    form.style.display = 'none';
    success.classList.add('visible');
    window.scrollTo({ top: success.offsetTop - 120, behavior: 'smooth' });
  } else {
    btn.disabled = false;
    alert('Something went wrong. Please try again or email us directly.');
  }
});
```

---

## Step 3 — Domain 白名單設定（推薦）

1. 登入 Web3Forms dashboard（用 `sales@bighead.com.hk`）
2. 找到對應的 form，設定 **Allowed Domains**
3. 填入 `bighead.com.hk`（或你的實際 domain）
4. 儲存

---

## Step 4 — 測試

1. 在瀏覽器開啟 `contact.html`
2. 填寫測試資料（名字、真實 email、訊息）
3. 勾選同意條款，按 **Send message**
4. 應出現成功畫面
5. 檢查 `sales@bighead.com.hk` 收件匣，確認收到提交內容

---

## 執行狀態

- [ ] 申請 Web3Forms Access Key
- [ ] 確認 `sales@bighead.com.hk` 收到確認信並複製 Key
- [ ] 修改 contact.html（hidden input + JS submit handler）
- [ ] 設定 Domain 白名單
- [ ] 測試提交並確認收到電郵
