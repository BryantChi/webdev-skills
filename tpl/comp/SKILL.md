---
id: component_templates
name: component_templates
description: Button、Form、Card、Nav、Modal 等 UI 元件模板
---

# 🧩 元件模板

## 按鈕

### HTML + CSS

```html
<button class="btn btn-primary">主要按鈕</button>
<button class="btn btn-secondary">次要按鈕</button>
<button class="btn btn-outline">外框按鈕</button>
```

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.75rem 1.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-primary {
  background: var(--color-primary);
  color: white;
  border: none;
}

.btn-primary:hover {
  opacity: 0.9;
}

.btn-secondary {
  background: var(--color-gray-200);
  color: var(--color-gray-800);
  border: none;
}

.btn-outline {
  background: transparent;
  color: var(--color-primary);
  border: 1px solid var(--color-primary);
}
```

---

## 卡片

```html
<article class="card">
  <img src="image.jpg" alt="" class="card-image">
  <div class="card-body">
    <h3 class="card-title">標題</h3>
    <p class="card-text">描述內容...</p>
  </div>
</article>
```

```css
.card {
  background: white;
  border-radius: 1rem;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.card-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-body {
  padding: 1.5rem;
}

.card-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.card-text {
  color: var(--color-gray-600);
  line-height: 1.6;
}
```

---

## 表單

```html
<form class="form">
  <div class="form-group">
    <label for="email" class="label">Email</label>
    <input type="email" id="email" class="input" placeholder="your@email.com">
  </div>
  <div class="form-group">
    <label for="password" class="label">密碼</label>
    <input type="password" id="password" class="input">
  </div>
  <button type="submit" class="btn btn-primary">登入</button>
</form>
```

```css
.form-group {
  margin-bottom: 1rem;
}

.label {
  display: block;
  font-size: 0.875rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.input {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid var(--color-gray-300);
  border-radius: 0.5rem;
  font-size: 1rem;
}

.input:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}
```

---

## 導航列

```html
<nav class="navbar">
  <a href="/" class="navbar-brand">Logo</a>
  <ul class="navbar-nav">
    <li><a href="/" class="nav-link">首頁</a></li>
    <li><a href="/about" class="nav-link">關於</a></li>
    <li><a href="/contact" class="nav-link">聯絡</a></li>
  </ul>
</nav>
```

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background: white;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.navbar-brand {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--color-primary);
  text-decoration: none;
}

.navbar-nav {
  display: flex;
  gap: 2rem;
  list-style: none;
}

.nav-link {
  color: var(--color-gray-700);
  text-decoration: none;
}

.nav-link:hover {
  color: var(--color-primary);
}
```

---

## Modal

```html
<div class="modal-overlay" id="modal">
  <div class="modal">
    <div class="modal-header">
      <h2 class="modal-title">標題</h2>
      <button class="modal-close">&times;</button>
    </div>
    <div class="modal-body">
      <p>內容...</p>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary">取消</button>
      <button class="btn btn-primary">確認</button>
    </div>
  </div>
</div>
```

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  background: white;
  border-radius: 1rem;
  width: 100%;
  max-width: 500px;
  max-height: 90vh;
  overflow: auto;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
  border-bottom: 1px solid var(--color-gray-200);
}

.modal-body {
  padding: 1.5rem;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  padding: 1rem 1.5rem;
  border-top: 1px solid var(--color-gray-200);
}
```

---

## 相關技能

- [元件組合模式](../../comp/SKILL.md)（Compound / Slot / Headless 模式 — 設計新元件 API 時必看）
- [shadcn/ui + Radix](../../css/shadcn-radix.md)（取代部分自寫元件）
- [元件單元測試](../../test/unit-vitest.md)
- [元件 a11y 規則](../../test/a11y-axe.md)
- [元件視覺回歸](../../test/visual-regression.md)
- [動畫](../../anim/SKILL.md)（互動微動效）
- [表單元件](../../form/SKILL.md)（RHF + Zod）
- [Coding Style](../../code/SKILL.md)

> **設計新元件前**：先看 [`comp/compact.md`](../../comp/compact.md) 決定用 Compound / Slot / Headless 哪種模式。
