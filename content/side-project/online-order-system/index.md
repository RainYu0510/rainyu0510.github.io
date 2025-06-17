---
title: 線上點餐系統
date: 2025-05-26
draft: false
description: a description
tags:
  - HTML
  - JavaScript
  - CSS
summary: 線上點餐系統，支援分類、訂單、折扣與即時計算。

---

這是一套完整的 **線上點餐系統（Online Order System)** 網頁專案，包括 HTML、CSS 與 JavaScript，做於網頁前端練習。以下是功能與架構的詳細說明：


# 📁 專案結構

## 🧩 一、HTML (index.html) 

這是整體畫面的骨架，負責結構與內容位置。

### 頁面基本設定

```c
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <title>🍱 線上點餐系統</title>
  <link rel="stylesheet" href="style.css" />
</head>
```

- \<!DOCTYPE html\>：告訴瀏覽器這是 HTML5
- lang="zh-Hant"：語言設定為繁體中文
- \<link rel="stylesheet"\>：載入 CSS 檔案（外觀）


### 主標題與菜單篩選區塊

```c
<h1>🍱 線上點餐系統</h1>

<div id="menu-container">
  <h2>菜單</h2>
  <label for="category-select">分類篩選：</label>
  <select id="category-select">
    <option value="all">全部</option>
    <option value="main">主食</option>
    <option value="drink">飲料</option>
    <option value="dessert">甜點</option>
  </select>
  <div id="menu"></div>
</div>
```

- 提供使用者切換商品分類的功能。
- \<div id="menu"\>：JavaScript 會在這裡動態插入商品項目。


### 訂單與折扣功能

```c
<div id="order-container">
  <h2>您的訂單</h2>
  <div id="order"></div>

  <p>總金額：<span id="total">0</span> 元</p>

  <label for="discount-code">優惠折扣碼：</label>
  <input type="text" id="discount-code" placeholder="輸入折扣碼" />
  <button id="apply-discount">套用折扣</button>
  <p id="discount-message" style="color: red; margin-top: 5px;"></p>

  <button onclick="submitOrder()">送出訂單</button>
  <button onclick="clearOrder()" class="clear-button" style="margin-left: 10px;">清空訂單</button>
</div>
```

- 顯示訂單內容與計算總金額
- 支援折扣碼 輸入後更新金額 (SAVE10)
- 按鈕事件由 JavaScript 控制功能（送出/清空）


## 🎨 二、CSS (style.css)

### 全局樣式

```c
body {
  font-family: -apple-system, "Segoe UI", Roboto, sans-serif;
  background-color: #f2f2f7;
  color: #1c1c1e;
  padding: 30px;
  margin: 0;
}
```

- 設定背景色、字體與內邊距，讓整體風格乾淨一致。

### 卡片與排版

```c
#menu {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 24px;
}

.menu-item {
  background-color: #fafafa;
  border-radius: 16px;
  padding: 20px;
  text-align: center;
  box-shadow: 0 4px 12px rgba(0,0,0,0.04);
}
```

- 使用 CSS Grid 排列菜單，讓每筆商品呈現為一張卡片。

### 按鈕樣式

```c
button {
  background-color: #34c759;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 12px;
  font-size: 17px;
  cursor: pointer;
}
```

- 主按鈕採用綠色，圓角設計，清空按鈕另外用紅色標示。


## 🧠 三、JavaScript (script.js)

這部分是整個系統的「大腦」，控制邏輯與互動。

### 1. 商品資料與購物車變數

```c
const menu = [ ... ]; // 商品清單
const cart = {};      // 用於儲存使用者選購的商品（id: 數量）
let discount = 1;     // 預設折扣倍率為 1（無折扣）
```

- 資料為固定陣列，未使用 API。
- cart 用來儲存用戶選擇的商品與數量。

### 2. 渲染菜單（renderMenu）

```c
function renderMenu(filter = 'all') {
  const filteredMenu = filter === 'all' ? menu : menu.filter(item => item.category === filter);
  ...
}
```

- 根據分類篩選菜單（例如只顯示 main 類）
- 用 forEach 把每筆商品變成 HTML 顯示

### 3. 數量輸入與更新（updateOrder）

```c
function updateOrder(id, quantity) {
  if (quantity > 0) cart[id] = parseInt(quantity);
  else delete cart[id];
  renderOrder();
  renderMenu(document.getElementById('category-select').value);
}
```

- 輸入數量會即時更新購物車
- 再重新渲染訂單與菜單數字同步

### 渲染訂單與金額（renderOrder）

```c
function renderOrder() {
  let total = 0;
  for (let id in cart) {
    const item = menu.find(m => m.id == id);
    const subtotal = cart[id] * item.price;
    total += subtotal;
  }
  totalSpan.textContent = Math.round(total * discount);
}
```

- 依據 cart 中的品項計算金額
- 套用 discount 折扣倍率後顯示

### 5. 折扣碼處理

```c
document.getElementById('apply-discount').addEventListener('click', () => {
  const code = document.getElementById('discount-code').value.trim().toUpperCase();
  if (code === 'SAVE10') {
    discount = 0.9;
    ...
  }
});
```

- 偵測使用者輸入 SAVE10 折扣碼
- 成功會改為 9 折並更新顯示金額

### 6. 訂單送出與清空功能

```c
function submitOrder() {
  if (Object.keys(cart).length === 0) {
    alert("您尚未選擇任何品項！");
    return;
  }
  alert("訂單已送出！");
  clearOrder();
}
```

- 檢查是否選購商品，若有則顯示「訂單已送出」並清空

---c
function clearOrder() {
  for (let id in cart) delete cart[id];
  discount = 1;
  document.getElementById('discount-code').value = '';
  renderOrder();
  renderMenu(document.getElementById('category-select').value);
}
---

- 清空購物車與折扣碼欄位
- 回復所有初始畫面狀態


# ✅ 總結：程式邏輯流程圖（簡要）

- 選擇分類 → renderMenu() → 顯示商品

- 數量變更 → updateOrder() → 修改購物車 + renderOrder()

- 輸入折扣碼 → applyDiscount → 套用折扣 → renderOrder()

- 點擊送出 → submitOrder() → 檢查 → alert → clearOrder()
