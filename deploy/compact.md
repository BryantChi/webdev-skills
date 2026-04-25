---
id: web_deployment_compact
name: web_deployment_compact
description: 部署平台選擇 + 必設項目 + CI 最低要求
---

# 部署精煉版

## 平台選擇

| 平台 | 適用 | 免費額度 | 強項 |
|------|------|---------|------|
| **Vercel** | Next.js、靜態、Edge | 慷慨 | DX 最佳、Preview URL |
| **Netlify** | Static、Jamstack | 慷慨 | Forms / Functions 整合 |
| **Cloudflare Pages** | 靜態 + Workers | 慷慨 | CDN 全球、Workers 邊緣運算 |
| **AWS Amplify** | 全 AWS 生態 | 中等 | 與 AWS 整合 |
| **Docker + 自架** | 完全控制 | — | 客製化、合規 |
| **Coolify / Dokku** | 開源 PaaS | — | 自架 PaaS |

## 平台決策表

| 條件 | 推薦 |
|------|------|
| Next.js + 不想煩惱 | Vercel |
| 純靜態 / Jamstack | Cloudflare Pages / Netlify |
| 要長時間執行 cron / queue | Docker / Render |
| 合規要求自架 | Docker / k8s |
| 全球 CDN + 邊緣運算 | Cloudflare |

## CRITICAL 規則（不限平台）

| # | Rule |
|---|---|
| D1 | 所有 secret 用 platform secret manager，不進 git |
| D2 | 區分 dev/staging/prod 環境變數 |
| D3 | preview URL 加 `noindex` + auth |
| D4 | 部署前 build success 再 deploy（CI gate） |
| D5 | 部署後跑 smoke test（`/api/health`） |
| D6 | 設 alert（部署失敗、5xx 飆升） |
| D7 | rollback 策略（一鍵或上版備份） |
| D8 | 域名 HTTPS（Let's Encrypt 自動） |

## 環境變數最小集

```
# 公開
NEXT_PUBLIC_APP_URL=https://example.com
NEXT_PUBLIC_GA_ID=...

# 機密（server only）
DATABASE_URL=...
NEXTAUTH_SECRET=...
SENTRY_DSN=...

# 區分環境
NODE_ENV=production
APP_ENV=production # staging / production
```

## CI 最低標準

每次 PR / push 都要跑：

| 階段 | 指令 |
|------|------|
| 1. install | `npm ci` |
| 2. lint | `npm run lint` |
| 3. type | `npm run typecheck` |
| 4. test | `npm test` |
| 5. build | `npm run build` |
| 6. e2e | `npm run e2e:ci`（PR 跑、push main 跑） |
| 7. lighthouse | `lhci autorun`（main 跑） |

## Anti-Pattern

| Bad | Good |
|---|---|
| 部署用 `git pull` 進機器 | CI/CD 流程化 |
| 直接改 prod env file | 用 secret manager |
| 部署無回滾計畫 | 上版前備份 / blue-green |
| 用 master 分支跑 | 用 main + tag / release branch |
| 同一 `.env` 開發/正式共用 | 嚴格分環境 |
