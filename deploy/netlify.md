---
id: web_deployment_netlify
name: web_deployment_netlify
description: Netlify 部署完整指南（靜態 / SSG / Edge Functions）
---

# Netlify

## 快速開始

```bash
npm i -g netlify-cli
netlify login
netlify init
netlify deploy --prod
```

## `netlify.toml`

```toml
[build]
  command = "npm run build"
  publish = "dist"  # 或 .next（@netlify/plugin-nextjs）

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/old-path"
  to = "/new-path"
  status = 301

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200  # SPA fallback

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "SAMEORIGIN"
    X-Content-Type-Options = "nosniff"
    Referrer-Policy = "strict-origin-when-cross-origin"

[[headers]]
  for = "/_next/static/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"
```

## CRITICAL 規則

| # | Rule |
|---|---|
| NL1 | Next.js 用 `@netlify/plugin-nextjs` 而非 vendor lock |
| NL2 | Form 處理用內建 Forms（spam protection 內建） |
| NL3 | Edge Functions 用 Deno，注意 API 差異 |
| NL4 | branch deploy 設密碼保護或 noindex |
| NL5 | 機密用 Site env vars，不入 git |

## Functions

```ts
// netlify/functions/hello.ts
import type { Handler } from '@netlify/functions';

export const handler: Handler = async (event) => {
  return { statusCode: 200, body: JSON.stringify({ message: 'Hi' }) };
};
```

呼叫：`/.netlify/functions/hello`。

## Edge Functions（Deno runtime）

```ts
// netlify/edge-functions/auth.ts
export default async (req: Request) => {
  const cookie = req.headers.get('cookie') || '';
  if (!cookie.includes('session=')) {
    return Response.redirect(new URL('/login', req.url));
  }
};

export const config = { path: '/dashboard/*' };
```

## Forms（無後端表單）

```html
<form name="contact" method="POST" data-netlify="true">
  <input type="hidden" name="form-name" value="contact">
  <input name="email" type="email" required>
  <textarea name="message" required></textarea>
  <button type="submit">送出</button>
</form>
```

提交內容自動進 Netlify Forms dashboard。

## 與 Vercel 比較

| 項目 | Netlify | Vercel |
|---|---|---|
| Next.js DX | 好（plugin） | 最佳（原生） |
| 純靜態 | 極佳 | 極佳 |
| Forms 內建 | ✅ | ❌ |
| Edge runtime | Deno | V8 |
| 免費額度 | 慷慨 | 慷慨 |

## Anti-Pattern

| Bad | Good |
|---|---|
| 用 Netlify 跑 Next.js 還自接 server | 用官方 plugin |
| Forms 不加 honeypot | 加 `netlify-honeypot` |
| `_redirects` 寫一堆規則 | 用 `netlify.toml` 結構化 |
