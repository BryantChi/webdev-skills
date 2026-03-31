---
id: responsive_design
name: responsive_design
description: 斷點、Mobile-First 與多裝置適配指南
---

# 📱 響應式設計 (RWD)

## 斷點系統

### 標準斷點

| 名稱 | 寬度 | 裝置 |
|-----|------|-----|
| xs | < 576px | 手機 |
| sm | ≥ 576px | 大手機 |
| md | ≥ 768px | 平板 |
| lg | ≥ 1024px | 小筆電 |
| xl | ≥ 1280px | 桌機 |
| 2xl | ≥ 1536px | 大螢幕 |

### CSS 變數

```css
:root {
  --breakpoint-sm: 576px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
  --breakpoint-2xl: 1536px;
}
```

---

## Mobile First

```css
/* 基礎樣式 = 手機 */
.container {
  padding: 1rem;
}

/* 平板以上 */
@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
}

/* 桌機以上 */
@media (min-width: 1024px) {
  .container {
    max-width: 1024px;
    margin: 0 auto;
  }
}
```

---

## 響應式網格

```css
.grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
}

@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

---

## 流體字型

```css
:root {
  --text-h1: clamp(2rem, 5vw, 3.5rem);
  --text-h2: clamp(1.5rem, 4vw, 2.5rem);
  --text-body: clamp(1rem, 2vw, 1.125rem);
}
```

---

## 響應式圖片

```html
<picture>
  <source media="(min-width: 1024px)" srcset="large.jpg">
  <source media="(min-width: 768px)" srcset="medium.jpg">
  <img src="small.jpg" alt="描述">
</picture>
```

```css
img {
  max-width: 100%;
  height: auto;
}
```

---

## 觸控友善

```css
/* 最小觸控目標 44x44px */
button, a {
  min-height: 44px;
  min-width: 44px;
}

/* 足夠的間距 */
.nav-item + .nav-item {
  margin-left: 1rem;
}
```

---

## 測試

1. **Chrome DevTools**：裝置模式
2. **實機測試**：手機、平板
3. **BrowserStack**：多裝置測試
4. **Lighthouse**：效能檢測
