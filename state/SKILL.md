---
id: web_state_management
name: web_state_management
description: 狀態管理選型（Zustand / TanStack Query / Pinia / Redux Toolkit）。不含資料層 ORM
---

# 🗂️ 狀態管理

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 選型決策樹 + 必裝 |
| L2 | [zustand.md](zustand.md) | Zustand（推薦中小） |
| L2 | [tanstack-query.md](tanstack-query.md) | Server state |
| L2 | [redux-toolkit.md](redux-toolkit.md) | RTK（大型嚴謹） |
| L2 | [pinia.md](pinia.md) | Vue 生態 |

## 觸發關鍵字

state、狀態、global、store、Zustand、Redux、TanStack Query、SWR、Pinia、Jotai、context

## Verification

- 是否分清四種狀態（local / URL / server / global）？
- Server state 是否用 TanStack Query 而非 useEffect？

見 [`../_shared/anti-patterns.md`](../_shared/anti-patterns.md) 資料流段。
