---
id: compact_guide
name: compact_guide
description: Token 優化版本（~2500 tokens），適用於 token 受限的 AI 工具
---

# ⚡ 精簡版核心指南

> 此檔案濃縮了整個 Skills 系統的核心。執行任務時請先讀 [`_shared/load-paths.md`](_shared/load-paths.md) 決定要載哪些檔，**不要全載**。

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

| 需求 | 前端 | CSS | 後端 | 部署 |
|-----|------|-----|------|------|
| SEO 重要 | Next.js | Tailwind + shadcn | - | Vercel |
| SPA | React + Vite | Tailwind | - | Cloudflare Pages |
| 快速開發 | Vue / Nuxt | Tailwind | Laravel | Vercel / Render |
| 完整後台 | Next.js | shadcn | Node/Laravel | Vercel + DB |
| 純靜態 | Astro | Tailwind | - | Cloudflare Pages |

→ 規則庫：[fe/*-rules.critical.md](fe/) ｜ shadcn：[css/shadcn-radix.md](css/shadcn-radix.md)

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

| 類別 | 必過 | 詳細 |
|------|------|------|
| 基礎 | HTTPS、title、description、alt、對比 4.5:1、無 console error | [check/launch.md](check/launch.md) |
| 效能 | LCP ≤2.5s、INP ≤200ms、CLS ≤0.1、Lighthouse ≥90 | [check/lighthouse.md](check/lighthouse.md) |
| 無障礙 | axe-core 通過、WCAG 2.2 AA、鍵盤可達 | [check/a11y.md](check/a11y.md) |
| SEO | 每頁 metadata、sitemap、JSON-LD、canonical | [check/seo.md](check/seo.md) |

---

## 🔬 品質工程速查

| 主題 | 起點 | 詳細 |
|------|------|------|
| 效能 / Core Web Vitals | [perf/compact.md](perf/compact.md) | perf/cwv, lighthouse, bundle |
| 測試（E2E + 單元 + a11y） | [test/compact.md](test/compact.md) | test/e2e-playwright, unit-vitest, a11y-axe |
| 技術 SEO | [seo/compact.md](seo/compact.md) | seo/meta-og, schema, sitemap-robots, canonical |
| 元件組合 | [comp/compact.md](comp/compact.md) | comp/compound, slot, headless |
| 多語系 | [i18n/compact.md](i18n/compact.md) | i18n/next-intl, vue-i18n |
| 部署 | [deploy/compact.md](deploy/compact.md) | deploy/vercel, netlify, docker, ci-github |
| 監控 | [obs/compact.md](obs/compact.md) | obs/sentry, web-vitals-reporter, logging |

## 🎯 應用功能速查（New）

| 主題 | 起點 | 詳細 |
|------|------|------|
| 狀態管理 | [state/compact.md](state/compact.md) | state/zustand, tanstack-query, redux-toolkit, pinia |
| 表單體系 | [form/compact.md](form/compact.md) | form/react-hook-form, tanstack-form, server-actions-zod, file-upload |
| 認證授權 | [auth/compact.md](auth/compact.md) | auth/auth-js, clerk, jwt-cookie, rbac-abac |
| 動畫互動 | [anim/compact.md](anim/compact.md) | framer-motion（React 微互動）、gsap（複雜時間軸）、view-transitions（頁面切換） |

---

## 📖 需要更多細節？

按需載入（先看 [`_shared/load-paths.md`](_shared/load-paths.md)）：
- 設計系統：`ui/color.md`, `ui/typo.md`, `ui/space.md`
- 特定風格：`ui/styles/XX-name.md`
- 特定框架：`fe/react.md`, `be/laravel.md`、`fe/*-rules.critical.md`
- 完整流程：`workflows/build-website.md`（已升級為 15 階段）

## 🌐 規則優先級

CRITICAL（必修）→ HIGH（強建議）→ MEDIUM（review 載入）→ LOW（風格偏好）
詳見 [`_shared/priority-legend.md`](_shared/priority-legend.md)
通用反模式速查 [`_shared/anti-patterns.md`](_shared/anti-patterns.md)
