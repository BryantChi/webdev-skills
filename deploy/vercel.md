---
id: web_deployment_vercel
name: web_deployment_vercel
description: Vercel 部署（Next.js 與靜態）完整指南
---

# Vercel

## 快速開始

```bash
npm i -g vercel
vercel link
vercel env pull .env.local
vercel --prod
```

或在 GitHub 連結：每個 push 自動 preview，main 自動 prod。

## `vercel.json`

```json
{
  "regions": ["sin1", "hnd1"],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "SAMEORIGIN" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" },
        { "key": "Permissions-Policy", "value": "camera=(), microphone=(), geolocation=()" }
      ]
    },
    {
      "source": "/_next/static/(.*)",
      "headers": [{ "key": "Cache-Control", "value": "public, max-age=31536000, immutable" }]
    }
  ],
  "redirects": [
    { "source": "/old-path", "destination": "/new-path", "permanent": true }
  ]
}
```

## CRITICAL 規則

| # | Rule | 說明 |
|---|---|---|
| VC1 | 用 Vercel CLI 拉 env：`vercel env pull` | 開發/正式對齊 |
| VC2 | preview deployments 設密碼或 `noindex` | 防被搜尋引擎收 |
| VC3 | 機密只設 Production scope，不要 All | 防 preview 洩漏 |
| VC4 | 用 Edge Functions 處理低延遲（A/B 測試、auth） | 邊緣運算 |
| VC5 | ISR 設 `revalidate` 避免每次 SSR | 效能 + 成本 |
| VC6 | Image Optimization 自動，不用第三方 | 內建 |
| VC7 | 域名加 www / apex 重定向 | 統一 canonical |

## Cron Jobs

```json
// vercel.json
{
  "crons": [
    { "path": "/api/cron/cleanup", "schedule": "0 2 * * *" }
  ]
}
```

```ts
// app/api/cron/cleanup/route.ts
import { NextResponse } from 'next/server';

export async function GET(req: Request) {
  if (req.headers.get('authorization') !== `Bearer ${process.env.CRON_SECRET}`) {
    return new Response('Unauthorized', { status: 401 });
  }
  // 清理邏輯
  return NextResponse.json({ ok: true });
}
```

## Edge / Serverless / Static 三選

| 類型 | 適用 | 範例 |
|------|------|------|
| Static | 文章、首頁 | `export const dynamic = 'force-static'` |
| Serverless | 一般 API、認證 | 預設 |
| Edge | 低延遲、A/B、middleware | `export const runtime = 'edge'` |

## 監控與分析

- Vercel Analytics（內建 RUM、Web Vitals）
- Vercel Speed Insights
- Logs：CLI `vercel logs <deployment>`

## Preview 工作流

```bash
git checkout -b feat/x
git push origin feat/x
# Vercel 自動產 preview URL
# 在 PR 留言看 URL
```

## 限制注意

| 項目 | Hobby | Pro |
|---|---|---|
| Serverless function 執行 | 10s | 60s（可調 300s） |
| Edge function 執行 | 30s | 30s |
| Image transform / month | 1000 | 5000 |
| Bandwidth | 100GB | 1TB |

> 若需要長時間執行（>60s），考慮 background job 服務（Inngest、Trigger.dev）。

## Anti-Pattern

| Bad | Good |
|---|---|
| 把 build 在 GitHub Actions 跑再 deploy | 直接 push 給 Vercel build |
| 用 `next start` self-host | 直接用 Vercel |
| 不設 region | 設離使用者最近 |
| 在 preview 用 prod DB | 用 staging DB |
