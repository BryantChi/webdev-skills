---
id: web_testing_compact
name: web_testing_compact
description: 測試體系精煉版：金字塔比例、必裝工具、最低覆蓋
---

# 測試體系精煉版

## 金字塔（建議比例）

| 層 | 工具 | 比例 | 速度 |
|---|---|---|---|
| 單元 | Vitest / Jest | 70% | 毫秒級 |
| 整合 | Vitest + Testing Library | 20% | 秒級 |
| E2E | Playwright | 10% | 數十秒 |

## 必裝清單（前端專案）

| 任務 | 套件 |
|---|---|
| 單元 + DOM | `vitest @testing-library/{react,vue} jsdom` |
| E2E | `@playwright/test` |
| a11y in test | `@axe-core/playwright` |
| MSW（API mock） | `msw` |
| 視覺回歸 | Playwright 內建 `toHaveScreenshot` |

## CRITICAL 規則

| # | Rule | 範例 |
|---|---|---|
| T1 | 不在 unit test 裡 mock 整個資料庫 | 用測試 DB / fixtures |
| T2 | E2E 用 page object / fixture，不重複 selector | `page.getByRole('button')` |
| T3 | 不用 `sleep` / 固定 timeout | 用 `await page.waitForResponse` |
| T4 | a11y 檢查整合進 E2E | 每頁跑一次 axe |
| T5 | PR 必跑 critical 路徑 E2E | smoke + happy path |

## 最低覆蓋

- 關鍵業務流程 100%（登入、結帳、表單送出）
- 元件 ≥ 80%（公開 API + 邊界條件）
- 不追求行覆蓋率，看**意圖覆蓋**

## Anti-Pattern

| Bad | Good |
|---|---|
| 把整個 store mock 掉 | 用真 store + 假資料 |
| `expect(x).toBeTruthy()` 模糊 | 對具體值 |
| 測試中等 random / time | inject clock / seed |
| 測 implementation detail | 測使用者可見行為 |
