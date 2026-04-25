---
id: web_performance_cwv
name: web_performance_cwv
description: Core Web Vitals (LCP/INP/CLS) 詳細規則與修補手冊
---

# Core Web Vitals 修補手冊

## LCP（Largest Contentful Paint）≤ 2.5s

**LCP 元素通常是 hero 圖、大標題或首屏關鍵內容。**

| Priority | 規則 | 範例 |
|---|---|---|
| CRITICAL | 用 `<img fetchpriority="high">` 標記 LCP 圖 | `<img src="/hero.avif" fetchpriority="high" width="1200" height="600">` |
| CRITICAL | LCP 圖**不要** `loading="lazy"` | 移除 `loading="lazy"` 在首屏 |
| CRITICAL | 預連 CDN：`<link rel="preconnect">` | 對字型、API、圖床 |
| CRITICAL | 字型 preload + `font-display: swap` | `<link rel="preload" as="font" type="font/woff2" crossorigin>` |
| HIGH | 用 AVIF / WebP 替代 PNG/JPG | next/image 自動 |
| HIGH | 圖片 `srcset` + `sizes`（responsive） | 避免下載過大尺寸 |
| HIGH | TTFB 過高 → CDN / Edge / cache | Cloudflare、Vercel Edge |
| MEDIUM | inline 首屏 critical CSS | `next` 自動，Vite 用 plugin |

### 量測

```bash
npx lighthouse https://yourdomain --only-categories=performance --view
```

DevTools → Performance → 重新整理 → 看 LCP 標記。

## INP（Interaction to Next Paint）≤ 200ms

**取代 FID 為新指標（2024 起）**。

| Priority | 規則 | 範例 |
|---|---|---|
| CRITICAL | 拆 long task（>50ms） | `requestIdleCallback`、`scheduler.postTask` |
| CRITICAL | Hydration 不阻塞 | Next App Router、Astro Islands |
| CRITICAL | 大 list 用 virtualization | TanStack Virtual |
| HIGH | debounce / throttle 高頻事件 | input、scroll |
| HIGH | `useTransition` 標記非緊急 update | 避免 input 卡 |
| HIGH | 把重運算搬到 Web Worker | comlink |
| MEDIUM | 移除無用 `useEffect` | 減少 re-render |

### 量測

DevTools → Performance → 互動後看 INP。Chrome 113+ 內建。

## CLS（Cumulative Layout Shift）≤ 0.1

**任何「跳版」都會累積 CLS。**

| Priority | 規則 | 範例 |
|---|---|---|
| CRITICAL | 圖片必設 width/height 或 aspect-ratio | `<img width="800" height="600">` |
| CRITICAL | 字型 swap 用 `size-adjust` 對齊 metrics | `next/font` 自動處理 |
| CRITICAL | 不在內容上方插入元素 | banner 從下方滑入而非從上推下來 |
| HIGH | embed (YouTube/Tweet) 預留容器 | 固定高度 placeholder |
| HIGH | 動態廣告位預留尺寸 | reserve slot height |
| MEDIUM | skeleton 與實際內容尺寸接近 | 避免 skeleton → content 跳 |

### 量測

DevTools → Performance → Layout Shift 圖層紅框。

## 真實使用者數據（RUM）

光 Lighthouse 不夠，**52% 的網站 lab 過、field 沒過**。建議：

| 工具 | 用途 |
|------|------|
| `web-vitals` npm | 收 client 端 LCP/INP/CLS 上報 |
| Vercel Analytics | 內建 |
| Cloudflare Web Analytics | 免費 |
| CrUX Dashboard | 看 Google 真實數據 |

```ts
import { onLCP, onINP, onCLS } from 'web-vitals';
onLCP(({ value }) => analytics.track('LCP', { value }));
onINP(({ value }) => analytics.track('INP', { value }));
onCLS(({ value }) => analytics.track('CLS', { value }));
```

## Anti-Pattern

見 [`../_shared/anti-patterns.md`](../_shared/anti-patterns.md) 的 DOM/渲染段。
