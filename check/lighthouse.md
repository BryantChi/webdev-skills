---
id: lighthouse_checklist
name: lighthouse_checklist
description: Lighthouse 上線前檢查清單與快速修補對照
---

# 🚀 Lighthouse 檢查清單

> 詳細規則見 [`../perf/SKILL.md`](../perf/SKILL.md)

## 目標分數（Production）

| 類別 | 最低 | 目標 |
|------|------|------|
| Performance | 80 | 95 |
| Accessibility | 90 | 100 |
| Best Practices | 90 | 100 |
| SEO | 90 | 100 |

## Core Web Vitals 預算

- [ ] LCP ≤ 2.5s（mobile）
- [ ] INP ≤ 200ms
- [ ] CLS ≤ 0.1
- [ ] TTFB ≤ 800ms

## 上線前必過項目

### 圖片
- [ ] LCP 圖加 `fetchpriority="high"`、不 `loading="lazy"`
- [ ] 所有圖設 `width` / `height` 或 `aspect-ratio`
- [ ] 用 AVIF / WebP，next/image / nuxt-image
- [ ] 加 srcset / sizes（responsive）

### 字型
- [ ] `font-display: swap`
- [ ] preload 關鍵字型
- [ ] subset 只載中英常用
- [ ] 用 `next/font`（Next）/ `nuxt-fonts`（Nuxt）

### JS / Bundle
- [ ] 初始 JS ≤ 150KB（gzip）
- [ ] 大型套件 dynamic import
- [ ] 第三方 script 用 `defer` / lazy load
- [ ] tree-shake 過（lodash → lodash-es 單函式）

### 快取
- [ ] hashed assets `Cache-Control: public, max-age=31536000, immutable`
- [ ] HTML `no-cache` 或短 max-age
- [ ] 用 CDN（Cloudflare / Vercel Edge）

### Render
- [ ] 首屏資料 SSR / RSC，不 CSR fetch
- [ ] 並行 fetch（Promise.all）
- [ ] 拆 Suspense boundary（streaming）
- [ ] 大 list virtualization

## 快速修補表

| 失分項 | 一句修補 |
|---|---|
| LCP 圖太慢 | preload + fetchpriority="high" + AVIF |
| CLS 高 | 圖片設 width/height，banner 不從上推 |
| Largest layout shift = font | `next/font` 自動處理 size-adjust |
| TBT/INP 高 | 拆 long task、改 RSC、移除無用 effect |
| Render-blocking resources | CSS critical inline、JS defer |
| Unused JS | dynamic import、tree-shake、移除 polyfill |
| Document not minified | 開 production build |

## CI 整合

```bash
npx @lhci/cli@latest autorun
```

`.lighthouserc.json` 設 budgets，超出 fail。

## RUM（Real User Monitoring）

光 Lighthouse 不夠，**52% 網站 lab 過、field 沒過**。安裝：

```bash
npm i web-vitals
```

```ts
import { onLCP, onINP, onCLS } from 'web-vitals';
[onLCP, onINP, onCLS].forEach((fn) => fn(({ name, value }) => analytics.track(name, { value })));
```

## 連向

- 詳細：[`../perf/cwv.md`](../perf/cwv.md), [`../perf/lighthouse.md`](../perf/lighthouse.md), [`../perf/bundle.md`](../perf/bundle.md)
