---
id: next_rules_optional
name: next_rules_optional
description: Next.js 進階規則（MEDIUM + LOW）；review/refactor 才載入
---

# Next.js 規則 — MEDIUM + LOW

## MEDIUM

| # | Rule | 說明 |
|---|---|---|
| NM1 | 自訂 `Link` wrapper 統一處理 prefetch 策略 | 大量列表時關 prefetch |
| NM2 | 用 `(group)/` 區隔 marketing / app / dashboard layout | 結構清晰 |
| NM3 | 用 `parallel routes` 實作 modal | URL 可分享 |
| NM4 | 環境變數分層：`.env.local` / `.env.production` | 部署正確 |
| NM5 | i18n 用 `next-intl` 或 routing 內建 | App Router 友善 |
| NM6 | 每頁 metadata 寫獨特 description | SEO |
| NM7 | dynamic import 設好 loading 元件 | 體驗 |

## LOW

| # | Rule |
|---|---|
| NL1 | route handler 一個檔案一個動詞（GET/POST 同檔） |
| NL2 | server action 命名動詞開頭（createUser, deletePost） |
| NL3 | 用 `tsx` 不用 `js` |
| NL4 | shadcn/ui 元件放 `components/ui/`，業務元件放 `components/` |

## 進階主題

- shadcn/ui → [`../css/shadcn-radix.md`](../css/shadcn-radix.md)
- 效能 → [`../perf/SKILL.md`](../perf/SKILL.md)
- SEO → [`../seo/SKILL.md`](../seo/SKILL.md)
- 測試 → [`../test/SKILL.md`](../test/SKILL.md)
