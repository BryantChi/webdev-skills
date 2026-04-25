---
id: frontend_frameworks
name: frontend_frameworks
description: React、Vue、Next.js、Nuxt.js、Vanilla JS 前端框架指南
---

# 🖥️ 前端框架技能

## 框架選擇指南

| 框架 | 類型 | 適用場景 | 學習曲線 |
|-----|------|---------|---------|
| [React](react.md) | 函式庫 | SPA、複雜應用 | 中等 |
| [Vue](vue.md) | 框架 | 中小型專案、漸進式 | 較低 |
| [Next.js](next.md) | React 框架 | SSR/SSG、SEO 需求 | 中等 |
| [Nuxt.js](nuxt.md) | Vue 框架 | SSR/SSG、SEO 需求 | 中等 |
| [Vanilla](vanilla.md) | 原生 | 簡單網站、效能優先 | 低 |

---

## 快速選擇

### 依專案需求

| 需求 | 推薦 |
|-----|------|
| SEO 重要 | Next.js / Nuxt.js |
| SPA 應用 | React / Vue |
| 快速開發 | Vue / Nuxt.js |
| 大型團隊 | React + TypeScript |
| 學習成本低 | Vue |
| 靜態網站 | Next.js / Nuxt.js / Astro |

### 依技術背景

| 背景 | 推薦 |
|-----|------|
| 新手 | Vue |
| 熟悉 React | Next.js |
| 熟悉 Vue | Nuxt.js |
| 後端開發者 | Vue / Laravel + Inertia |

---

## 共通最佳實踐

### 專案結構

```
src/
├── components/       # 可重用元件
│   ├── ui/          # 基礎 UI 元件
│   └── common/      # 共用元件
├── pages/ or views/ # 頁面
├── layouts/         # 佈局
├── hooks/ or composables/ # 邏輯復用
├── services/ or api/     # API 呼叫
├── stores/          # 狀態管理
├── utils/           # 工具函式
├── styles/          # 全域樣式
└── assets/          # 靜態資源
```

### 命名規範

| 類型 | 規範 | 範例 |
|-----|------|-----|
| 元件 | PascalCase | `UserCard.vue` |
| 頁面 (Next) | kebab-case | `user-profile.tsx` |
| 函式 | camelCase | `getUserData()` |
| 常數 | UPPER_SNAKE | `API_BASE_URL` |
| CSS class | kebab-case / BEM | `user-card__title` |

### 效能優化

1. **程式碼分割**：動態載入
2. **圖片優化**：使用 next/image 或 nuxt-image
3. **快取策略**：適當的快取設定
4. **Bundle 分析**：定期檢查 bundle 大小

---

## 規則庫（按優先級分檔，預設只載 critical）

| 框架 | 必修（CRITICAL+HIGH） | 進階（MEDIUM+LOW，review 才載） |
|------|--------------------|-------------------------------|
| React | [react-rules.critical.md](react-rules.critical.md) | [react-rules.optional.md](react-rules.optional.md) |
| Vue | [vue-rules.critical.md](vue-rules.critical.md) | [vue-rules.optional.md](vue-rules.optional.md) |
| Next | [next-rules.critical.md](next-rules.critical.md) | [next-rules.optional.md](next-rules.optional.md) |

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

---

## 相關技能

- [CSS 架構](../css/SKILL.md) ｜ [shadcn/Radix](../css/shadcn-radix.md)
- [Coding Style](../code/SKILL.md)
- [響應式設計](../rwd/SKILL.md)
- [元件組合模式](../comp/SKILL.md)
- [效能優化](../perf/SKILL.md)
- [測試體系](../test/SKILL.md)
- [技術 SEO](../seo/SKILL.md)
