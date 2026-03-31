---
name: Compact Guide
description: Token 優化版本（~2000 tokens），適用於 token 受限的 AI 工具
---

# ⚡ 精簡版核心指南

> 此檔案濃縮了整個 Skills 系統的核心，約 ~2000 tokens，適合一次載入

---

## 🧠 核心邏輯 (Reasoning First)

**開發前必想：**
1. **意圖**：SaaS? 電商? 針對誰?
2. **反模式**：銀行不用霓虹色，律師不用可愛字。
3. **設計決策**：先決定風格與配色，再寫 Code。

---

## 🎯 核心原則

### 去 AI 感（必遵守）

```
❌ 禁止：完美對稱、庫存照片、「專業領先」空話、三個一樣的卡片、安全藍配色
✅ 要求：適度非對稱、真實照片、具體文案、有記憶點、大膽配色
```

---

## 🎨 風格快速參考

| # | 風格 | 配色 | 適用 |
|---|-----|------|-----|
| 01 | 極簡 | 黑白灰 | 作品集、高端 |
| 04 | 玻璃擬態 | 漸層+透明 | 科技、現代 |
| 08 | 暗黑 | 深色+亮點 | 科技、遊戲 |
| 11 | 企業 | 藍白 | B2B、官網 |
| 13 | 電商 | 橙/紅CTA | 購物 |
| 14 | SaaS | 紫藍漸層 | 軟體產品 |
| 37 | 北歐 | 米色+木 | 生活、家居 |
| 43 | 賽博龐克 | 霓虹+黑 | 遊戲、娛樂 |
| 52 | 奢華 | 黑金 | 高端品牌 |

完整 55 種：詳見 `ui/styles/index.md`

---

## 🔧 技術快速選擇

| 需求 | 前端 | CSS | 後端 |
|-----|------|-----|------|
| SEO 重要 | Next.js | Tailwind | - |
| SPA | React | Tailwind | - |
| 快速開發 | Vue | Tailwind | Laravel |
| 完整後台 | Next.js | Tailwind | Node/Laravel |

---

## 📐 設計 Token

```css
/* 核心變數 - 複製使用 */
:root {
  /* 色彩 - 依風格替換 */
  --primary: #3b82f6;
  --secondary: #64748b;
  --bg: #ffffff;
  --text: #1e293b;
  
  /* 間距 */
  --space-4: 1rem;
  --space-8: 2rem;
  --space-16: 4rem;
  
  /* 圓角 */
  --radius: 8px;
  
  /* 陰影 */
  --shadow: 0 4px 6px rgba(0,0,0,0.1);
  
  /* 字型 */
  --font: 'Inter', sans-serif;
}
```

---

## 🧩 核心元件模板

### 按鈕

```css
.btn {
  background: var(--primary);
  color: white;
  padding: 12px 24px;
  border-radius: var(--radius);
  font-weight: 500;
}
```

### 卡片

```css
.card {
  background: var(--bg);
  border-radius: calc(var(--radius) * 2);
  padding: var(--space-8);
  box-shadow: var(--shadow);
}
```

---

## ✅ 上線檢查（精簡版）

- [ ] HTTPS
- [ ] 每頁有 title + description
- [ ] 圖片有 alt
- [ ] 行動版測試
- [ ] 對比度 4.5:1+
- [ ] 無 console 錯誤

---

## 📖 需要更多細節？

按需載入：
- 設計系統：`ui/color.md`, `ui/typo.md`, `ui/space.md`
- 特定風格：`ui/styles/XX-name.md`
- 特定框架：`fe/react.md`, `be/laravel.md` 等
- 完整流程：`workflows/build-website.md`
