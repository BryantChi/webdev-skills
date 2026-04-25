---
id: web_observability
name: web_observability
description: Web 可觀測性（Sentry 錯誤追蹤、web-vitals RUM、結構化 logging）
---

# 📡 可觀測性

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 必裝四件套 + 預算 |
| L2 | [sentry.md](sentry.md) | 錯誤追蹤 + Replay |
| L2 | [web-vitals-reporter.md](web-vitals-reporter.md) | RUM 真實使用者數據 |
| L2 | [logging.md](logging.md) | 結構化 logging 與聚合 |

## 觸發關鍵字

監控、observability、Sentry、Datadog、log、error、RUM、web-vitals、alert

## 與其他模組

- 效能：[`../perf/cwv.md`](../perf/cwv.md) 解釋指標含義
- 部署：[`../deploy/SKILL.md`](../deploy/SKILL.md) 設 alert
