---
id: web_animation_compact
name: web_animation_compact
description: 動畫選型決策（CSS / JS / View Transitions）+ 必設規則
---

# 動畫精煉版

## 選型決策

| 場景 | 推薦 |
|------|------|
| Hover / focus state、簡單 transition | **CSS transitions** |
| 進場動畫、staggered reveal | **CSS @keyframes** + `animation-delay` |
| 滾動觸發 + 時間軸 | **GSAP ScrollTrigger** |
| 元件 layout 變動（FLIP） | **Framer Motion `layout` prop** |
| 路由切換動畫 | **View Transitions API**（2026 主流支援） |
| 共享元素轉場 | **View Transitions** 或 Framer Motion |
| 向量動畫（複雜 SVG） | **Lottie**（After Effects 匯出） |
| 3D | **Three.js / R3F** |

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| AN1 | 必支援 `prefers-reduced-motion` | 一律播 | `@media (prefers-reduced-motion: reduce)` 關掉 |
| AN2 | 用 `transform` / `opacity` 動畫，不用 `top`/`left`/`width` | 觸發 layout | composite-only |
| AN3 | 不在動畫期間阻塞 main thread（重運算）| INP 飆 | Web Worker |
| AN4 | 自然曲線：`cubic-bezier(0.34, 1.56, 0.64, 1)` 或 `ease-out` | 預設 ease | 自訂彈性 |
| AN5 | staggered reveal 用 delay step，不要平均散落 | 雜亂 | 80-120ms 步進 |
| AN6 | scroll 動畫上設 `passive: true` listener 或用 IntersectionObserver | 卡頓 | passive / IO |
| AN7 | 動畫元素設 `will-change` 但用完移除 | 永遠 will-change | 動完移除 |
| AN8 | 大型套件 dynamic import | 全包進 bundle | `dynamic(import, { ssr: false })` |

## 動畫哲學（對齊 frontend-design）

> **重點不是動畫數量，而是時機和編排**。

- 高影響時刻：頁面初次載入（staggered reveal）、scroll 進入、重要互動
- 一個精心編排的進場動畫 > 散落 10 個 hover 微動畫
- 「克制 + 時機」勝過「氾濫」

## 必設 reduced motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

或在 JS：

```ts
const reduce = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
if (!reduce) playAnimation();
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 整頁滿是 Framer Motion | 只用在關鍵時刻 |
| `transition: all 0.3s ease` 預設 | 指定屬性 + 自訂曲線 |
| 動畫 width/height（layout shift） | transform: scale |
| 不支援 reduced motion | 必加 |
| 動畫期間 setState 過頻 | rAF 節流 |
| Lottie 用 1MB JSON 在首屏 | dynamic import + lazy |
