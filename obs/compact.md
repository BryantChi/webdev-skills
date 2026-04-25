---
id: web_observability_compact
name: web_observability_compact
description: 可觀測性必裝四件套 + 採樣策略
---

# 可觀測性精煉版

## 必裝四件套

| 類型 | 工具（推薦） | 替代 |
|------|------------|------|
| 錯誤追蹤 | Sentry | Bugsnag、Honeybadger |
| RUM（Web Vitals） | Vercel Analytics / Cloudflare RUM | `web-vitals` + 自建 |
| 結構化 log | pino / winston + Datadog / Logtail | console + Cloud logging |
| Uptime | Better Stack / UptimeRobot | Cron-based ping |

## CRITICAL 規則

| # | Rule | 說明 |
|---|---|---|
| O1 | 至少裝 Sentry + RUM | 兩者互補 |
| O2 | 採樣率設好（非 100%） | 控成本 |
| O3 | log 不含 PII（信用卡、密碼、token） | 隱私 + 法規 |
| O4 | Source map 上傳給 Sentry，但不公開到 client | debug 必要 |
| O5 | 設 alert 並接 Slack / PagerDuty | 主動知曉 |
| O6 | error 加 user context（非 PII，如 user id hash） | 易 triage |
| O7 | release 標 git SHA，方便溯源 | 連動部署 |

## 採樣策略建議

| 信號 | Production 採樣 | Staging |
|------|----------------|---------|
| Error events | 100% | 100% |
| Trace（performance） | 10-20% | 100% |
| Replay（Session） | 1-5%（含 100% on error） | 100% |
| Web Vitals | 100% | 100% |
| Logs（debug） | 1% | 100% |
| Logs（warn/error） | 100% | 100% |

## 必設 Alert

- 5xx 錯誤率 > 1%（5 分鐘）
- 部署後新錯誤類型出現
- LCP > 4s（field, 50p）
- 健康檢查連續 3 次失敗
- 第三方 API 失敗率上升

## Anti-Pattern

| Bad | Good |
|---|---|
| `console.log` 留在 prod | 用 logger，prod 過濾 level |
| Sentry 100% trace | 採樣降成本 |
| log 含 password / token | redact / 不寫 |
| alert 沒人值班 | 接 oncall 輪值 |
| error 不加 fingerprint | Sentry 自動，但驗證合理 |
