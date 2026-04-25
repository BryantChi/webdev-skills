---
id: react_rules_critical
name: react_rules_critical
description: React/Next 必修規則（CRITICAL + HIGH）；MEDIUM/LOW 見 react-rules.optional.md
---

# React 規則 — CRITICAL + HIGH

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)
> 通用反模式見 [`../_shared/anti-patterns.md`](../_shared/anti-patterns.md)

## CRITICAL（必修）

| # | Rule | Bad | Good |
|---|---|---|---|
| R1 | 不在 useEffect 取首屏資料 | `useEffect(() => fetch(...))` | RSC / TanStack Query / loader |
| R2 | 不串行 await（waterfall） | `const a = await x(); const b = await y(a.id)` 但 b 不需要 a | `Promise.all([x(), y()])` |
| R3 | list key 用穩定 ID，不是 index | `items.map((x, i) => <X key={i} />)` | `<X key={x.id} />` |
| R4 | useState 初始化用 lazy | `useState(JSON.parse(localStorage.x))` | `useState(() => JSON.parse(...))` |
| R5 | 不在 render 中 mutate state | `obj.x = 1; setState(obj)` | `setState({...obj, x: 1})` |
| R6 | useEffect deps 完整 | `useEffect(() => f(x), [])` | `[x]`（或重構） |
| R7 | 不條件呼叫 hook | `if (a) useState(...)` | hook 永遠頂層 |
| R8 | onClick 不傳 inline async | 失誤吞 error | 包 `void` 或處理 promise |
| R9 | 大 list 必虛擬化（>100 items） | `items.map` 全渲 | TanStack Virtual / react-window |
| R10 | 表單用 controlled + 驗證 schema | 散落 useState | RHF + Zod |

## Next.js App Router CRITICAL

| # | Rule | 範例 |
|---|---|---|
| N1 | 預設 server component，需要才 `'use client'` | client 越少 hydration 越快 |
| N2 | 在 server component 取資料，不要 client useEffect | `await fetch()` 直接寫 |
| N3 | 動態路由用 streaming + Suspense | `loading.tsx` + `<Suspense>` |
| N4 | metadata 用 `generateMetadata`，不在 client `<title>` | SEO 必設 |
| N5 | 圖片用 `next/image` + width/height | LCP / CLS |
| N6 | 不在 client component 把 server 資料當 props 傳整包 | 切 server boundary |
| N7 | route handler 不要回未驗證資料 | Zod 驗 input |
| N8 | env 機密只用 server，不暴露 `NEXT_PUBLIC_` | 滲透風險 |

## HIGH

| # | Rule | 修補 |
|---|---|---|
| R11 | 過度 `useMemo`/`useCallback` | 先 profile，必要才加 |
| R12 | Context 拆分（讀寫分離） | 兩個 Context 避免全局 re-render |
| R13 | error boundary 包關鍵區塊 | `<ErrorBoundary>` |
| R14 | suspense boundary 拆細 | 不要整頁 fallback |
| R15 | 不把 ref 當 reactive 來源 | 變動不觸發 render |
| R16 | 圖片 `priority`（首屏）/ `loading="lazy"`（次屏） | LCP 優化 |
| R17 | 第三方 script 用 `next/script strategy="lazyOnload"` | 不阻塞 |
| R18 | 表格大資料用 virtualized + sticky header | 體驗 |
| R19 | dialog/menu 用 Radix（a11y 內封） | 比自寫 a11y 更可靠 |
| R20 | Loading state 三態（idle/loading/error） | 不只 loading |

## 驗證

| 工具 | 抓什麼 |
|------|------|
| `eslint-plugin-react-hooks` | R6, R7 |
| `eslint-plugin-react` | R3, JSX a11y |
| `@next/eslint-plugin-next` | N1-N5 |
| TypeScript strict | R5 mutation |
| React DevTools Profiler | R11 過度 memo |

```bash
npm i -D eslint-plugin-react-hooks @next/eslint-plugin-next eslint-plugin-jsx-a11y
```
