---
id: web_observability_sentry
name: web_observability_sentry
description: Sentry 錯誤追蹤 + Performance + Session Replay 完整指南
---

# Sentry

## 安裝（Next.js）

```bash
npx @sentry/wizard@latest -i nextjs
```

Wizard 會建立：
- `sentry.client.config.ts`
- `sentry.server.config.ts`
- `sentry.edge.config.ts`
- `next.config.js` 包 `withSentryConfig`

## 核心設定（client）

```ts
// sentry.client.config.ts
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.APP_ENV ?? 'production',
  release: process.env.NEXT_PUBLIC_APP_VERSION,

  tracesSampleRate: 0.1,             // 10% trace
  replaysSessionSampleRate: 0.01,    // 1% session
  replaysOnErrorSampleRate: 1.0,     // error 必錄

  integrations: [
    Sentry.replayIntegration({ maskAllText: true, blockAllMedia: true }),
  ],

  beforeSend(event) {
    // 不送 dev / 過濾敏感
    if (process.env.NODE_ENV !== 'production') return null;
    if (event.user?.email) delete event.user.email;
    return event;
  },

  ignoreErrors: [
    'ResizeObserver loop limit exceeded',
    /chunk load failed/i,
  ],
});
```

## 標 release（CI 整合）

```yaml
# .github/workflows/deploy.yml
- name: Sentry release
  uses: getsentry/action-release@v1
  env:
    SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
    SENTRY_ORG: my-org
    SENTRY_PROJECT: my-project
  with:
    environment: production
    sourcemaps: ./.next
```

## CRITICAL 規則

| # | Rule | 說明 |
|---|---|---|
| ST1 | release 用 git SHA 標 | 對應部署 |
| ST2 | source map 上傳但**不公開到 client** | `next.config.js` 的 `hideSourceMaps: true` |
| ST3 | `replaysOnErrorSampleRate: 1.0` | error 必錄 replay |
| ST4 | replay 用 `maskAllText` / `blockAllMedia` | 隱私 |
| ST5 | `beforeSend` 過濾 PII 與 dev | 法規 |
| ST6 | 用 user context（id hash） + tag | 易 triage |

## 設 user context

```ts
// 登入後
Sentry.setUser({ id: hashUserId(user.id), role: user.role });

// 登出
Sentry.setUser(null);
```

## 自訂 breadcrumb / scope

```ts
Sentry.addBreadcrumb({ category: 'checkout', message: 'Click pay button', level: 'info' });

try { /* ... */ }
catch (e) {
  Sentry.withScope((scope) => {
    scope.setTag('feature', 'checkout');
    scope.setContext('cart', { items: cart.length, total: cart.total });
    Sentry.captureException(e);
  });
}
```

## Performance（Tracing）

Sentry 自動 trace `fetch`、route changes。手動加：

```ts
const span = Sentry.startInactiveSpan({ name: 'expensive-task' });
await doWork();
span.end();
```

## Alert 設定（在 Sentry UI）

| 條件 | 動作 |
|------|------|
| 新錯誤類型出現 | Slack #alerts |
| 5xx > 1% (5m) | PagerDuty oncall |
| 特定 issue 累計 100 events | Email |
| Performance：p75 LCP > 4s | Slack |

## 與 Vue / Nuxt

```bash
npm i @sentry/vue   # 或 @sentry/nuxt
```

```ts
import * as Sentry from '@sentry/vue';
Sentry.init({ app, dsn: '...', /* ... */ });
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 100% trace（成本高） | 10-20% |
| Replay 不 mask 敏感欄位 | mask |
| 不上傳 source map | 上傳但 hide from client |
| 上 dev / preview env 也送 | 用 environment 過濾 |
| 用 console.error 取代 | `Sentry.captureException` |
