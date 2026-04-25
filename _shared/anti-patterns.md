---
id: anti_patterns
name: anti_patterns
description: 跨框架/語言的通用反模式速查，各模組可引用以避免重複列舉
---

# 通用反模式速查

## 資料流

| Anti-Pattern | 為何不好 | 替代方案 |
|---|---|---|
| `useEffect` 內 fetch 資料 | 額外 round-trip、async waterfall | RSC / TanStack Query / SWR |
| 序列 await 取多筆資料 | waterfall 拖慢 LCP | `Promise.all` 或並行 RSC |
| 在 render 內計算昂貴值 | 每次 render 重算 | `useMemo` / `useState(init)` lazy |
| 全域狀態存所有事 | 牽一髮動全身 | 區域 state + URL state + server state 分層 |

## DOM / 渲染

| Anti-Pattern | 為何不好 | 替代方案 |
|---|---|---|
| `index` 當 list key | reorder 時重建 DOM | 穩定唯一 ID |
| layout 中放動態 inline style | 觸發 layout thrash | CSS variables / class 切換 |
| `<img>` 無 width/height | CLS 飆升 | 給定固定比例或 `aspect-ratio` |
| 大 list 無虛擬化 | 卷動卡頓 | TanStack Virtual / react-window |

## 樣式

| Anti-Pattern | 替代方案 |
|---|---|
| Hardcode 顏色 `#3b82f6` | 用 design token / CSS var |
| `!important` 滿天飛 | 重整選擇器特異性 |
| 重複定義 spacing 數值 | 統一 spacing scale |
| 千篇一律 8px 間距 | 有意識地變化（呼吸感） |

## 表單

| Anti-Pattern | 替代方案 |
|---|---|
| 受控元件無 schema 驗證 | React Hook Form + Zod |
| submit 後才驗證 | 即時驗證 + onBlur |
| 缺 `<label>` 關聯 | `<label htmlFor>` 或 `aria-label` |

## 後端 / API

| Anti-Pattern | 替代方案 |
|---|---|
| ORM N+1 | eager load / `with()` / `include` |
| 同步阻塞 I/O | async / queue / streaming |
| 把秘鑰寫進 repo | env + secret manager |
| API 無版本 | `/v1/` 路由前綴 |

## a11y

| Anti-Pattern | 替代方案 |
|---|---|
| `<div onClick>` 當按鈕 | `<button>` |
| 無焦點樣式 | `:focus-visible` 顯著輪廓 |
| 顏色作為唯一資訊 | 加 icon / 文字輔助 |
| 自動播放音訊 | 預設靜音或不自動播 |

## 引用方式

各模組規則檔：「除以下框架特定規則外，亦請套用 [`_shared/anti-patterns.md`](../_shared/anti-patterns.md)」
