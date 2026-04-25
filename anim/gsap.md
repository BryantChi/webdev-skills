---
id: web_animation_gsap
name: web_animation_gsap
description: GSAP — 時間軸 + ScrollTrigger 強型動畫（適合複雜編排）
---

# GSAP

## 何時選 GSAP

- **時間軸（timeline）編排複雜**：多步驟、條件、嵌套
- **ScrollTrigger 滾動體驗**：橫向、釘選、scrub
- **SVG 動畫**：morphing、繪製
- **跨框架共用**：Vue / Solid / React 都能用

> Framer Motion 對 React 元件動畫最順，GSAP 對「整體編排」最強。兩者可並用。

## 安裝

```bash
npm i gsap
```

GSAP 3.13+ 全部插件免費（包含原 Club GreenSock 收費品如 SplitText、MorphSVG）。

## 基本動畫

```ts
import { gsap } from 'gsap';

gsap.to('.box', {
  x: 200,
  rotation: 360,
  duration: 1,
  ease: 'power3.out',
});
```

## Timeline（時間軸）

```ts
const tl = gsap.timeline({ defaults: { duration: 0.6, ease: 'power2.out' } });

tl.from('.hero h1', { opacity: 0, y: 40 })
  .from('.hero p', { opacity: 0, y: 20 }, '-=0.3')   // 提前 0.3s
  .from('.hero button', { opacity: 0, scale: 0.9 }, '-=0.2')
  .from('.hero img', { opacity: 0, x: 50 }, '<');    // 與上一段同時開始
```

## ScrollTrigger（最強用途）

```ts
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);

// 進入視窗時觸發
gsap.from('.feature', {
  opacity: 0,
  y: 60,
  stagger: 0.1,
  scrollTrigger: {
    trigger: '.features',
    start: 'top 80%',
    toggleActions: 'play none none reverse',
  },
});

// 釘選 + scrub
gsap.to('.parallax', {
  y: -200,
  scrollTrigger: {
    trigger: '.parallax-section',
    start: 'top top',
    end: '+=500',
    scrub: 1,
    pin: true,
  },
});
```

## React 整合（推薦 useGSAP）

```bash
npm i @gsap/react
```

```tsx
import { useGSAP } from '@gsap/react';
import { useRef } from 'react';

export function Hero() {
  const root = useRef<HTMLDivElement>(null);

  useGSAP(() => {
    gsap.from('.hero-title', { opacity: 0, y: 40, duration: 0.8 });
  }, { scope: root });

  return <div ref={root}><h1 className="hero-title">Hello</h1></div>;
}
```

`useGSAP` 自動 cleanup（取消所有動畫），不會 memory leak。

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| GS1 | React 內用 `useGSAP` 而非 useEffect | 自動 cleanup |
| GS2 | ScrollTrigger 用 scope/context 避免漏 cleanup | 換頁不卡 |
| GS3 | 用 GSAP timeline 編排，不要疊一堆 setTimeout | 可控、可預覽 |
| GS4 | scroll scrub 動畫設 `scrub: 1` (數字) 而非 true | 1 秒平滑跟隨 |
| GS5 | 大量 ScrollTrigger 用 `batch` API | 性能 |
| GS6 | reduced motion：`gsap.timeline({ paused: reduced })` | 必處理 |
| GS7 | dynamic import GSAP 模組 | 不阻塞 LCP |

## 與 React 路由整合

```tsx
// 換頁前後重新計算 ScrollTrigger
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { usePathname } from 'next/navigation';

const pathname = usePathname();
useEffect(() => {
  ScrollTrigger.refresh();
}, [pathname]);
```

## SplitText（標題逐字動畫）

```ts
import { SplitText } from 'gsap/SplitText';
gsap.registerPlugin(SplitText);

const split = new SplitText('h1', { type: 'chars' });
gsap.from(split.chars, { opacity: 0, y: 50, stagger: 0.03, duration: 0.6, ease: 'power2.out' });
```

## GSAP vs Framer Motion

| 項 | GSAP | Framer Motion |
|---|---|---|
| 時間軸編排 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| ScrollTrigger | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| React 元件動畫 | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Layout / FLIP | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| SVG morph | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| 3D（MotionPath） | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| 框架無關 | ⭐⭐⭐⭐⭐ | ⭐⭐（純 React） |
| Bundle size | 中（~30KB） | 中（~38KB / Lazy 6KB） |

**並用建議**：作品集 / 品牌站 / 廣告頁 → GSAP 主導；React 應用 UI 動畫 → Framer Motion 主導。

## Anti-Pattern

| Bad | Good |
|---|---|
| useEffect 內呼叫 GSAP 不 cleanup | useGSAP 內封 |
| ScrollTrigger 路由切換不 refresh | 換頁後 `ScrollTrigger.refresh()` |
| `scrub: true` 抖動 | `scrub: 1`（平滑）|
| 大量元素用個別 ScrollTrigger | 用 `batch` API |
| 不處理 reduced motion | 必判 |
