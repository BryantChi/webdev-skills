---
id: web_observability_logging
name: web_observability_logging
description: 結構化 logging 與聚合（pino / winston / OpenTelemetry）
---

# 結構化 Logging

## 為何要結構化（JSON）

- 易聚合搜尋（Datadog、Logtail、Loki、CloudWatch）
- 易過濾（按 user id、request id）
- 易解析（不用正則）

## Node.js：pino（推薦，最快）

```bash
npm i pino pino-http
```

```ts
// src/lib/logger.ts
import pino from 'pino';

export const logger = pino({
  level: process.env.LOG_LEVEL ?? 'info',
  formatters: {
    level: (label) => ({ level: label }),
  },
  redact: {
    paths: ['password', 'token', 'authorization', '*.password', '*.creditCard'],
    censor: '[REDACTED]',
  },
  timestamp: pino.stdTimeFunctions.isoTime,
});
```

## HTTP middleware

```ts
import pinoHttp from 'pino-http';
import { randomUUID } from 'crypto';

export const httpLogger = pinoHttp({
  logger,
  genReqId: (req) => req.headers['x-request-id'] || randomUUID(),
  customProps: (req) => ({ userId: req.user?.id }),
  customLogLevel: (_req, res) => {
    if (res.statusCode >= 500) return 'error';
    if (res.statusCode >= 400) return 'warn';
    return 'info';
  },
});
```

## 用法

```ts
logger.info({ userId, orderId }, 'Order created');
logger.warn({ feature: 'checkout', latencyMs: 1200 }, 'Slow checkout');
logger.error({ err, userId }, 'Failed to charge');
```

## CRITICAL 規則

| # | Rule | 說明 |
|---|---|---|
| L1 | 用結構化 JSON，不 plain text | 易聚合 |
| L2 | redact 敏感欄位（password、token、信用卡、PII） | 法規 + 安全 |
| L3 | 加 request id 串連同次請求 log | trace |
| L4 | 加 user id（hash 或 id 不含 PII） | triage |
| L5 | level 分明（debug/info/warn/error）| prod 過 info |
| L6 | error log 必含 stack | debug |
| L7 | 不在熱路徑寫超大 object | 性能 |

## 聚合平台選擇

| 平台 | 適合 | 計費 |
|------|------|------|
| Datadog | 大型、APM 整合 | 貴 |
| Logtail / Better Stack | 中小型 | 親民 |
| Grafana Loki + Promtail | 自架 | 免費（自管） |
| AWS CloudWatch | AWS 生態 | 中 |
| Vercel logs | Vercel 部署 | 內建（短保留） |
| Axiom | 開發者友善 | 親民 |

## 與 OpenTelemetry 整合

```bash
npm i @opentelemetry/api @opentelemetry/sdk-node @opentelemetry/auto-instrumentations-node
```

```ts
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';

new NodeSDK({
  instrumentations: [getNodeAutoInstrumentations()],
}).start();
```

讓 trace + log 共享 trace_id，跨服務追蹤。

## Edge runtime（Cloudflare Workers / Vercel Edge）

不能用 pino（Node API）。改：

```ts
const log = (level: string, msg: string, fields = {}) => {
  console.log(JSON.stringify({ level, msg, ...fields, ts: new Date().toISOString() }));
};
```

平台會自動收 console。

## 前端 logging

| 場景 | 方案 |
|------|------|
| 錯誤 | Sentry（自動） |
| 行為事件 | PostHog / Mixpanel / Plausible |
| 效能 | web-vitals（見 [web-vitals-reporter.md](web-vitals-reporter.md)） |
| Custom | `/api/log` endpoint + sendBeacon |

**不要**用 `console.log` 留 prod，client 端 console 不會自動聚合。

## Anti-Pattern

| Bad | Good |
|---|---|
| `console.log('user', user)` 整物件吐 | logger.info({ userId }, 'message') |
| log 沒 redact 密碼 | redact 設定 |
| level 全 info | 細分 |
| log 內含完整 SQL（可能有 PII） | 過濾 |
| 同步 log 寫檔（阻塞 event loop） | pino async 或 stdout |
