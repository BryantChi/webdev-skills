---
name: Vanilla JavaScript 指南
description: 原生 JavaScript 開發最佳實踐
---

# Vanilla JavaScript 指南

## 適用場景

- 簡單網站
- 效能優先
- 學習基礎
- 無框架需求

---

## 專案結構

```
project/
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── main.js
│   ├── utils.js
│   └── components/
└── assets/
    └── images/
```

---

## DOM 操作

```javascript
// 選取元素
const el = document.querySelector('.my-class');
const els = document.querySelectorAll('.items');

// 事件監聽
el.addEventListener('click', (e) => {
  console.log(e.target);
});

// 修改內容
el.textContent = '新文字';
el.innerHTML = '<span>HTML</span>';

// 修改樣式
el.style.color = 'red';
el.classList.add('active');
el.classList.remove('hidden');
el.classList.toggle('show');

// 建立元素
const newEl = document.createElement('div');
newEl.className = 'card';
newEl.textContent = '內容';
document.body.appendChild(newEl);
```

---

## Fetch API

```javascript
// GET 請求
async function getUsers() {
  try {
    const response = await fetch('/api/users');
    if (!response.ok) throw new Error('請求失敗');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}

// POST 請求
async function createUser(userData) {
  const response = await fetch('/api/users', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(userData),
  });
  return response.json();
}
```

---

## 模組化

```javascript
// utils.js
export function formatDate(date) {
  return new Date(date).toLocaleDateString('zh-TW');
}

export function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// main.js
import { formatDate, debounce } from './utils.js';
```

---

## 元件模式

```javascript
// components/Modal.js
export class Modal {
  constructor(options) {
    this.el = document.createElement('div');
    this.el.className = 'modal';
    this.el.innerHTML = `
      <div class="modal-backdrop"></div>
      <div class="modal-content">
        <h2>${options.title}</h2>
        <p>${options.content}</p>
        <button class="modal-close">關閉</button>
      </div>
    `;
    
    this.el.querySelector('.modal-close')
      .addEventListener('click', () => this.close());
  }
  
  open() {
    document.body.appendChild(this.el);
  }
  
  close() {
    this.el.remove();
  }
}
```

---

## 最佳實踐

1. **使用 ES6+ 語法**
2. **模組化程式碼**
3. **事件委派**
4. **async/await 處理非同步**
5. **避免全域變數**
