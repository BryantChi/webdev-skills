---
id: web_performance_lighthouse
name: web_performance_lighthouse
description: Lighthouse CI 整合、評分閥值與自動化檢查
---

# Lighthouse CI 整合

## 目標分數

| 類別 | 最低 | 目標 |
|------|------|------|
| Performance | 80 | 95 |
| Accessibility | 90 | 100 |
| Best Practices | 90 | 100 |
| SEO | 90 | 100 |
| PWA（可選） | — | 90 |

## 安裝與本地執行

```bash
npm i -D @lhci/cli
npx lhci autorun --collect.url=http://localhost:3000
```

## `.lighthouserc.json` 範例

```json
{
  "ci": {
    "collect": {
      "url": ["http://localhost:3000/", "http://localhost:3000/blog"],
      "numberOfRuns": 3,
      "settings": { "preset": "desktop" }
    },
    "assert": {
      "assertions": {
        "categories:performance": ["error", { "minScore": 0.9 }],
        "categories:accessibility": ["error", { "minScore": 0.95 }],
        "first-contentful-paint": ["warn", { "maxNumericValue": 2000 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 2500 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }]
      }
    },
    "upload": { "target": "temporary-public-storage" }
  }
}
```

## GitHub Actions 整合

```yaml
- uses: actions/checkout@v4
- run: npm ci && npm run build && npm run start &
- run: npx wait-on http://localhost:3000
- run: npx lhci autorun
```

## Anti-Pattern

| 錯誤 | 後果 | 修補 |
|---|---|---|
| 只跑一次取分數 | 數值飄 | `numberOfRuns: 3` 取中位 |
| 在開發模式跑 | source map、HMR 拉低 | 跑 production build |
| 只跑首頁 | 漏掉重型頁 | 把 dashboard / list 都納入 |
| 忽略 mobile preset | 行動端才是真實 | 預設 mobile，不要 desktop |

## 與 RUM 並用

Lighthouse 是 lab，CrUX / web-vitals 是 field。**兩個都要看**。
