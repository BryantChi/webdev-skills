---
id: web_form_system_compact
name: web_form_system_compact
description: 表單選型決策 + 必裝清單 + 核心規則
---

# 表單體系精煉版

## 選型決策

| 場景 | 推薦 | 為何 |
|------|------|------|
| React 通用 | **React Hook Form + Zod** | 最主流、生態好 |
| TS 型別最強 | TanStack Form | 完整型別推導 |
| Next App Router | RHF + zod-form-data + server action | RSC 友善 |
| Vue / Nuxt | VeeValidate + Zod / FormKit | Vue 生態 |
| 純靜態頁 / Jamstack | Netlify Forms / Formspree | 無後端 |

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| F1 | 用 schema 驗證（Zod / Yup） | 散落 if 檢查 | 一支 Zod schema |
| F2 | server 再驗一次 | 只 client 驗 | server 也驗 |
| F3 | label 用 `<label htmlFor>` 關聯 | placeholder 當 label | 真 label |
| F4 | 錯誤用 `aria-describedby` 關聯 | 視覺紅但 SR 無感 | aria 關聯 |
| F5 | required 視覺 + a11y 雙標記 | 只放 `*` | + `aria-required` |
| F6 | submit 後焦點移到結果 / 第一個錯誤 | 焦點原地 | 顯式 focus |
| F7 | 不在 submit 期間允許重複送出 | 連按 | disable + loading |
| F8 | 大表單分步驟 | 一頁吞 | wizard / steps |
| F9 | 檔案上傳用 presigned URL | 經自家 server 中轉大檔 | 直接 → S3/R2 |
| F10 | 表單 state 不放 global | Redux form | 表單 lib 自管 |

## 必裝清單

| 任務 | 套件 |
|------|------|
| React 表單 | `react-hook-form` + `zod` + `@hookform/resolvers` |
| 型別 schema | `zod` |
| Server action 表單 | `zod-form-data` |
| TanStack Form | `@tanstack/react-form` |
| Vue | `vee-validate` + `zod` 或 `@vee-validate/zod` |
| 檔案上傳 | `@uppy/core` 或自行 + presigned |

## Anti-Pattern

| Bad | Good |
|---|---|
| 受控 useState 滿天飛 | 用 RHF / TanStack Form |
| 只 client 驗 | server 也驗 |
| Submit 後不顯示錯誤位置 | 焦點 + 滾動 |
| placeholder 當 label | 真 label |
| 把表單 state 放 Redux | RHF 自管 |
| 重複送出無 disable | submit handler 加 `formState.isSubmitting` 判斷 |
