---
id: web_performance_bundle
name: web_performance_bundle
description: Bundle size 優化（code splitting、tree shaking、dynamic import）
---

# Bundle 優化

## 預算

| 類型 | 上限（gzip） |
|------|-------------|
| 首頁初始 JS | 150 KB |
| 整站 vendor chunk | 200 KB |
| 單一頁面 | +50 KB |
| 字型 (woff2) | 100 KB |

## CRITICAL 規則

| # | Rule | 範例 |
|---|------|------|
| B1 | 大型函式庫 dynamic import | `const Chart = dynamic(() => import('./Chart'), { ssr: false })` |
| B2 | 不要全包 lodash | `import debounce from 'lodash/debounce'` 或換 lodash-es |
| B3 | moment → date-fns / dayjs | 省 ~70KB |
| B4 | 重 UI 套件按需 import | `import { Button } from '@mui/material/Button'`（避免 barrel） |
| B5 | 圖片不打包進 JS | 用 `next/image` / public/ |
| B6 | 移除未用 polyfill | browserslist 鎖定現代瀏覽器 |

## HIGH 規則

| # | Rule |
|---|------|
| B7 | 第三方統計腳本延後（lazy） |
| B8 | 字型只載入用到的 weight/subset |
| B9 | SVG icon 用 sprite / inline，避免 100 個 request |
| B10 | 設 `compression: gzip/br` |

## 分析工具

| 工具 | 指令 |
|------|------|
| Next 內建 | `ANALYZE=true npx next build` |
| Vite | `npx vite-bundle-visualizer` |
| Webpack | `webpack-bundle-analyzer` |
| 通用 | [bundlephobia.com](https://bundlephobia.com) 查單一套件 |

## Tree Shaking 檢查清單

- [ ] `package.json` 有 `"sideEffects": false`（純函式庫）
- [ ] 用 ES module（`import/export`），非 CommonJS
- [ ] 不用 `import * as X`（破壞 tree shake）
- [ ] 用具名 export，避免 default 整包

## Anti-Pattern

| Bad | Good |
|---|---|
| `import _ from 'lodash'` | `import debounce from 'lodash/debounce'` |
| `import * as Icons from 'lucide-react'` | `import { Search } from 'lucide-react'` |
| 100 個 SVG 各一檔 | sprite / inline |
| 單一 chunk 走天下 | route-based splitting |
