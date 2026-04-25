---
id: vue_rules_critical
name: vue_rules_critical
description: Vue 3 / Nuxt 必修規則（CRITICAL + HIGH）
---

# Vue 規則 — CRITICAL + HIGH

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)
> 通用反模式見 [`../_shared/anti-patterns.md`](../_shared/anti-patterns.md)

## CRITICAL（必修）

| # | Rule | Bad | Good |
|---|---|---|---|
| V1 | 用 `<script setup>`，不用 Options API（新檔） | 冗長 | Composition + setup |
| V2 | reactive 物件不解構失去響應 | `const { x } = props` | `const { x } = toRefs(props)` |
| V3 | v-for 必有 `:key`（穩定 ID） | `:key="i"` | `:key="item.id"` |
| V4 | v-if / v-for 不同層 | 同一 element 兩者並用 | 拆 wrapper 或 computed 篩 |
| V5 | props 預設值用 factory（物件/陣列） | `default: []` | `default: () => []` |
| V6 | emit 必型別宣告 | 不宣告 | `defineEmits<{ change: [v: string] }>()` |
| V7 | 不直接修改 props | `props.x = 1` | emit 給父層改 |
| V8 | 大 list 必虛擬化 | 全渲 | `vue-virtual-scroller` |
| V9 | 表單用 schema 驗 | 散 ref | VeeValidate / FormKit + Zod |
| V10 | composable 命名 `use*` | `getUser()` | `useUser()` |

## Nuxt 3 CRITICAL

| # | Rule | 範例 |
|---|---|---|
| NU1 | 取資料用 `useFetch` / `useAsyncData`，不用 axios in setup | SSR 對齊 |
| NU2 | 路由 metadata 用 `definePageMeta` | SEO / middleware |
| NU3 | SEO 用 `useSeoMeta` | 自動處理 OG |
| NU4 | 機密只放 `runtimeConfig`，不用 `public.*` | 防外洩 |
| NU5 | 圖片用 `<NuxtImg>` 或 `nuxt-image` | LCP |
| NU6 | server route 在 `server/api/`，回傳驗 schema | 安全 |
| NU7 | 用 Pinia 管狀態，不用 Vuex | 官方推薦 |

## HIGH

| # | Rule | 說明 |
|---|---|---|
| V11 | computed 純函式，不副作用 | side effect 用 watch |
| V12 | watchEffect deps 自動，watch 顯式 | 視情況選 |
| V13 | provide/inject 配合 readonly | 防子層亂改 |
| V14 | scoped CSS 預設 | 避免污染 |
| V15 | dialog/menu 用 Headless UI Vue | a11y |
| V16 | Suspense + loading | 體驗 |
| V17 | 動畫用 `<Transition>` / `<TransitionGroup>` | 內建好 |
| V18 | TypeScript 嚴格 | strict + isolatedModules |

## 驗證

| 工具 | 抓什麼 |
|------|------|
| ESLint + `eslint-plugin-vue` | V3, V4, V5 |
| `vue-tsc --noEmit` | 型別 |
| Vetur / Volar | IDE 提示 |

```bash
npm i -D eslint-plugin-vue @vue/eslint-config-typescript vue-tsc
```
