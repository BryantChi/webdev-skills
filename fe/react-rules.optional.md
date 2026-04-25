---
id: react_rules_optional
name: react_rules_optional
description: React/Next 進階規則（MEDIUM + LOW）；只在 review/refactor 載入
---

# React 規則 — MEDIUM + LOW

> 預設不載入，僅在 Code Review / 重構任務時載入

## MEDIUM

| # | Rule | 說明 |
|---|---|---|
| RM1 | 元件 props ≤ 5 個 | 超過考慮拆 children / compound |
| RM2 | 自訂 hook 命名 `use*` | 規範 |
| RM3 | 不在 component 內定義其他 component | 每次 render 重建 |
| RM4 | 用 discriminated union 處理變體 | 比多個 boolean 安全 |
| RM5 | Form input 名稱與 schema 對齊 | RHF + Zod |
| RM6 | 同步狀態用 URL search params | shareable / back button |
| RM7 | 動畫用 CSS 優先，JS 必要才 | 效能 |
| RM8 | className 用 cn / clsx + tailwind-merge | 衝突解 |
| RM9 | 不用 default export 大型元件 | named 利於 IDE |
| RM10 | Server actions 用 zod-form-data 驗 | 安全 |

## LOW

| # | Rule | 說明 |
|---|---|---|
| RL1 | 引號統一（' or "） | Prettier 處理 |
| RL2 | import 排序 | `eslint-plugin-import` |
| RL3 | JSX 屬性排序 | 一致即可 |
| RL4 | 元件檔名 PascalCase | 規範 |
| RL5 | 自訂 hook 一個檔案一個 | 易找 |
| RL6 | `aria-*` 不省略 | a11y |

## 重構訊號

| 訊號 | 動作 |
|------|------|
| 元件 > 200 行 | 拆子元件或 hook |
| useEffect > 3 個 | 重新思考資料流 |
| props > 7 個 | 改 compound |
| 同邏輯重複 3 處 | 抽 hook |
| Context 含太多東西 | 拆多個 Context |

## 進階主題（連向其他模組）

- 元件組合 → [`../comp/SKILL.md`](../comp/SKILL.md)
- 效能 → [`../perf/SKILL.md`](../perf/SKILL.md)
- 測試 → [`../test/SKILL.md`](../test/SKILL.md)
