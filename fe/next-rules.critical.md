---
id: next_rules_critical
name: next_rules_critical
description: Next.js App Router 必修規則（CRITICAL + HIGH）
---

# Next.js App Router 規則 — CRITICAL + HIGH

> 此檔聚焦 Next 14/15 App Router 特有規則。React 共通規則見 [react-rules.critical.md](react-rules.critical.md)。

## Server / Client Boundary（CRITICAL）

| # | Rule | 範例 |
|---|---|---|
| NX1 | 預設 server，需 hooks/event 才 `'use client'` | 越下層越好 |
| NX2 | server 取資料 = `await fetch()` 直接寫 | 不要 useEffect |
| NX3 | 不要在 client 大量 import server-only 套件 | bundle 爆 |
| NX4 | 從 server → client 傳 props 用 serializable | Date / Map 不可序列化 |
| NX5 | 機密只在 server / route handler，env 不加 `NEXT_PUBLIC_` | 安全 |
| NX6 | 用 `server-only` 套件標記 server module | 防誤 import |

## 資料取得（CRITICAL）

| # | Rule | 範例 |
|---|---|---|
| NX7 | 並行 fetch 用 `Promise.all` | 避免 waterfall |
| NX8 | revalidate 策略明確（time / tag / on-demand） | `fetch(..., { next: { revalidate: 60 }})` |
| NX9 | 動態資料用 `dynamic = 'force-dynamic'` 或 `noStore()` | 避免錯誤快取 |
| NX10 | tag-based 失效用 `revalidateTag` | 比 revalidatePath 精準 |
| NX11 | mutation 用 server actions + `revalidateTag` | 完整資料流 |
| NX12 | server action 必驗 input（zod） | 不信任 client |

## 效能（CRITICAL）

| # | Rule |
|---|---|
| NX13 | 圖片必用 `next/image` + width/height（CLS）|
| NX14 | 字型必用 `next/font`（自動 optimize、subset、swap） |
| NX15 | 第三方 script 用 `next/script` + 適當 strategy |
| NX16 | 路由 streaming：用 `loading.tsx` + Suspense |
| NX17 | 大型 client component 用 `dynamic(import, { ssr: false })` |

## SEO / Metadata（CRITICAL）

| # | Rule | 範例 |
|---|---|---|
| NX18 | 用 `generateMetadata` 動態 | 不在 client 設 title |
| NX19 | sitemap 用 `app/sitemap.ts` | 自動產 |
| NX20 | robots 用 `app/robots.ts` | 自動產 |
| NX21 | OG image 用 `app/og/route.tsx` 動態產 | 1200x630 |

## 安全（CRITICAL）

| # | Rule |
|---|---|
| NX22 | route handler 必驗 input（zod） |
| NX23 | server action 內部驗權限 |
| NX24 | rate limit 用 middleware + KV / Redis |
| NX25 | CSP header 用 `headers()` 設好 |

## HIGH

| # | Rule |
|---|---|
| NX26 | parallel routes（`@modal`）做 modal 路由 |
| NX27 | intercepting routes（`(.)`）做行內預覽 |
| NX28 | route group `(group)/` 區隔 layout |
| NX29 | not-found.tsx / error.tsx 每層都加 |
| NX30 | middleware 只做 redirect / auth check，不重邏輯 |

## Anti-Pattern

| Bad | Good |
|---|---|
| 整頁 `'use client'` | 只在需要的葉子 |
| `useState` 存 server 資料 | 用 RSC + Suspense |
| 自寫 cache logic | 用 `unstable_cache` / fetch tags |
| middleware 內 fetch DB | 太重，改 route handler |

## 驗證

```bash
npx next lint
npx next build  # 看 bundle 大小、static / dynamic 標記
```
