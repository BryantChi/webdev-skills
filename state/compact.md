---
id: web_state_management_compact
name: web_state_management_compact
description: 狀態管理選型決策樹 + 四種狀態分層
---

# 狀態管理精煉版

## 四種狀態分層（先分清）

| 類型 | 範例 | 工具 |
|------|------|------|
| **Local UI state** | `isOpen`、`hover` | `useState` / `useReducer` |
| **URL state** | filter、page、tab | `useSearchParams` / router query |
| **Server state** | API 資料、cache | TanStack Query / SWR / RSC |
| **Global client state** | theme、user、cart | Zustand / Jotai / Pinia |

> **關鍵**：80% 的「全域 state 需求」其實是 Server state，用 TanStack Query 解決後就不需要 Redux。

## 選型決策樹

```
需要跨多個無關元件共享？
├─ 不需要 → useState / useReducer
└─ 需要：
   ├─ 是 server 資料？ → TanStack Query / SWR / RSC
   ├─ 是 URL 可分享？ → useSearchParams
   ├─ 是 global UI（theme / cart）？
   │   ├─ React 中小型 → Zustand
   │   ├─ 細粒度更新 → Jotai / Recoil
   │   ├─ 大型嚴謹（Redux DevTools 重要）→ Redux Toolkit
   │   └─ Vue → Pinia
   └─ 是表單？ → React Hook Form（不要丟 global）
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| ST1 | 不把 server 資料放 global store | `Redux store.users` | `useQuery(['users'])` |
| ST2 | 不把表單 state 放 global | Redux form | RHF / TanStack Form |
| ST3 | 不在 Context 塞所有東西 | 一個 AppContext | 拆 ThemeCtx / AuthCtx |
| ST4 | Context 讀寫分離 | 同 provider 內值 + setter | 兩個 Context |
| ST5 | URL 可代表的 state 放 URL | `useState(filter)` | `useSearchParams` |
| ST6 | server cache key 一致 | 散在各處 | 集中 query key factory |

## 必裝清單（建議）

| 場景 | 套件 |
|------|------|
| React 通用 | `zustand` + `@tanstack/react-query` |
| Next App Router | RSC + `@tanstack/react-query` (mutation) |
| Vue / Nuxt | `pinia` + `@tanstack/vue-query` |
| 大型企業 React | `@reduxjs/toolkit` + RTK Query |
| 細粒度 | `jotai` |

## Anti-Pattern

| Bad | Good |
|---|---|
| 全部塞 Redux | 分層：local / URL / server / global |
| useEffect + fetch + setState | TanStack Query |
| Context drilling 所有元件 | 局部 Context + Zustand global |
| Store 包整棵 form 狀態 | RHF |
