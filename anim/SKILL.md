---
id: web_animation
name: web_animation
description: Web 動畫（Framer Motion、GSAP、View Transitions API、Lottie）。CSS 動畫見 ui/no-ai-feel.md
---

# 🎬 Web 動畫

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)
> 動畫哲學見 [`../ui/no-ai-feel.md`](../ui/no-ai-feel.md)（cubic-bezier、staggered reveal）

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 何時 CSS / 何時 JS |
| L2 | [framer-motion.md](framer-motion.md) | Framer Motion（最主流） |
| L2 | [gsap.md](gsap.md) | GSAP（時間軸 + ScrollTrigger） |
| L2 | [view-transitions.md](view-transitions.md) | View Transitions API（瀏覽器原生） |

## 觸發關鍵字

動畫、animation、transition、scroll、motion、Framer Motion、GSAP、Lottie、View Transitions

## Verification

- 是否支援 `prefers-reduced-motion`？
- 動畫是否影響 INP / CLS？（見 [`../perf/cwv.md`](../perf/cwv.md)）
