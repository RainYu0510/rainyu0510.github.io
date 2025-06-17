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