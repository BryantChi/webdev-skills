---
id: web_performance_compact
name: web_performance_compact
description: 效能規則精煉版，適合預設載入；詳細範例見 cwv/lighthouse/bundle.md
---

# 效能規則精煉版

## Core Web Vitals 預算

| 指標 | Good | Needs Work | Poor |
|------|------|------------|------|
| LCP | ≤2.5s | ≤4.0s | >4.0s |
| INP | ≤200ms | ≤500ms | >500ms |
| CLS | ≤0.1 | ≤0.25 | >0.25 |
| TTFB | ≤800ms | ≤1.8s | >1.8s |

## CRITICAL 規則（必修）

| # | Rule | Bad | Good |
|---|------|-----|------|
| P1 | 圖片必設尺寸或 aspect-ratio | `<img src=...>` | `<img width height>` 或 `aspect-ratio: 16/9` |
| P2 | 首屏圖加 `fetchpriority="high"` + 不 lazy | `loading="lazy"` 在 hero | `fetchpriority="high"` |
| P3 | 字型 `font-display: swap` + preload | 預設 block | `<link rel="preload" as="font">` + `swap` |
| P4 | 避免 CSR 取首屏資料 | useEffect 取再渲染 | RSC / SSR / streaming |
| P5 | 並行而非序列 fetch | 連續 await | `Promise.all` |
| P6 | 第三方腳本延後 | head 同步 script | `defer` / `next/script strategy="lazyOnload"` |
| P7 | 不在 LCP 元素之上塞 modal/banner | 阻擋 LCP 渲染 | 改 deferred mount |

## HIGH 規則

| # | Rule | 修補 |
|---|------|------|
| P8 | bundle 過大（>200KB gzip） | dynamic import、tree shake |
| P9 | client component 過多（Next） | 預設 server component |
| P10 | 重型函式庫（moment、lodash 全包） | 換 date-fns、lodash-es 單函式 |
| P11 | 圖片格式落伍（PNG 大圖） | AVIF / WebP，next/image |
| P12 | 缺 cache header | `Cache-Control: public, max-age=31536000, immutable`（hashed assets） |

## 修補優先順序

1. 量化（Lighthouse / CrUX / RUM）
2. 找最大瓶頸（看 Performance tab 的 Long Task）
3. 修一個指標、再量
4. 不要一次改五個地方

## 詳細

→ [cwv.md](cwv.md) ｜ [lighthouse.md](lighthouse.md) ｜ [bundle.md](bundle.md)
