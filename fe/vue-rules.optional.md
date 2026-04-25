---
id: vue_rules_optional
name: vue_rules_optional
description: Vue 進階規則（MEDIUM + LOW）；review/refactor 才載入
---

# Vue 規則 — MEDIUM + LOW

## MEDIUM

| # | Rule |
|---|---|
| VM1 | 元件命名多字（`UserCard` 而非 `Card`）|
| VM2 | 單檔元件順序：template → script → style |
| VM3 | composable 一個檔案一個 |
| VM4 | 全域 mixin 不用，改 composable |
| VM5 | event 命名用 kebab-case（template）/ camelCase（emit） |
| VM6 | URL state 用 `useRoute` + query | 同步狀態 |
| VM7 | 同步資料用 `useStorage`（VueUse）| localStorage 響應式 |
| VM8 | i18n 用 `vue-i18n` |

## LOW

| # | Rule |
|---|---|
| VL1 | 元件名 PascalCase |
| VL2 | prop 在 template 用 kebab-case |
| VL3 | 自關閉空元素 `<MyComp />` |
| VL4 | template 屬性順序：is → v-for → v-if → ref → key → others |
| VL5 | 不在 template 寫複雜 expression，抽 computed |

## 重構訊號

| 訊號 | 動作 |
|------|------|
| `<script>` > 200 行 | 抽 composable |
| watch > 3 個 | 重新看資料流 |
| 多元件共用邏輯 | 抽 composable |
| Pinia store 含太多 | 拆多個 store |

## 連向其他模組

- 組合 → [`../comp/SKILL.md`](../comp/SKILL.md)
- 效能 → [`../perf/SKILL.md`](../perf/SKILL.md)
- 測試 → [`../test/SKILL.md`](../test/SKILL.md)
