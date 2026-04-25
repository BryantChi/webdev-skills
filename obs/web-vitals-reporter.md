---
id: web_observability_vitals
name: web_observability_vitals
description: Web Vitals RUM 上報（真實使用者效能數據）
---

# Web Vitals RUM

## 為何需要 RUM

- Lighthouse = lab 數據（理想條件）
- CrUX / RUM = field 數據（真實使用者）
- **52% 的網站 lab 過、field 沒過**

## 安裝

```bash
npm i web-vitals
```

## 上報範本

```ts
// src/lib/web-vitals.ts
import { onLCP, onINP, onCLS, onFCP, onTTFB } from 'web-vitals';

const send = (metric: { name: string; value: number; id: string; rating: string }) => {
  const body = JSON.stringify({
    ...metric,
    page: location.pathname,
    timestamp: Date.now(),
  });

  // 用 sendBeacon 確保頁面關閉也能送
  if (navigator.sendBeacon) {
    navigator.sendBeacon('/api/vitals', body);
  } else {
    fetch('/api/vitals', { body, method: 'POST', keepalive: true });
  }
};

export function reportWebVitals() {
  onLCP(send);
  onINP(send);
  onCLS(send);
  onFCP(send);
  onTTFB(send);
}
```

## Next.js 整合

```ts
// app/layout.tsx
'use client';
import { useEffect } from 'react';
import { reportWebVitals } from '@/lib/web-vitals';

export default function RootLayout({ children }) {
  useEffect(() => { reportWebVitals(); }, []);
  return <html>...</html>;
}
```

或用 Next 內建：

```ts
// app/_components/WebVitals.tsx
'use client';
import { useReportWebVitals } from 'next/web-vitals';

export function WebVitals() {
  useReportWebVitals((metric) => {
    fetch('/api/vitals', { body: JSON.stringify(metric), method: 'POST', keepalive: true });
  });
  return null;
}
```

## 接收 endpoint

```ts
// app/api/vitals/route.ts
import { NextResponse } from 'next/server';

export async function POST(req: Request) {
  const data = await req.json();
  // 寫進你的分析平台（PostHog、Datadog、Clickhouse）
  await db.vitals.create({ data });
  return NextResponse.json({ ok: true });
}
```

## 內建選項（不自架）

| 方案 | 備註 |
|------|------|
| Vercel Speed Insights | Next 部署在 Vercel 一鍵開啟 |
| Cloudflare Web Analytics | 免費、無 cookie |
| Google Search Console → Core Web Vitals | 看 CrUX field data |
| PostHog | 含 vitals + product analytics |
| Datadog RUM | 與 backend trace 串連 |

## CRITICAL 規則

| # | Rule | 說明 |
|---|---|---|
| WV1 | 用 `sendBeacon` / `keepalive: true` | 確保頁面關閉也送 |
| WV2 | 加 `page` / `device` / `connection` 維度 | 易切片 |
| WV3 | 看 **p75（field 75 百分位）** 而非平均 | 真實感 |
| WV4 | 與部署時間對齊（找回退） | 標 release |
| WV5 | 行動 / 桌機分開看 | 體驗差很多 |

## 監控儀表板必含

- LCP / INP / CLS p75（按 device）
- 趨勢線（每日）
- Top 5 慢的頁面
- 與部署事件對齊
- 與業務指標關聯（轉換率 vs LCP）

## Anti-Pattern

| Bad | Good |
|---|---|
| 看平均（mean） | 看 p75 / p95 |
| 不分 device / connection | 分維度切片 |
| 一次性截圖 | 持續追蹤趨勢 |
| 只看 LCP 不看 INP | 三項都看 |
