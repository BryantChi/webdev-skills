---
id: web_animation_framer
name: web_animation_framer
description: Framer Motion（motion/react）— React 動畫主流方案
---

# Framer Motion (motion/react)

> 套件名 2026 已更新為 `motion`（原 `framer-motion`），仍向下相容。

## 安裝

```bash
npm i motion
```

## 基本範例

```tsx
import { motion } from 'motion/react';

<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.5, ease: [0.34, 1.56, 0.64, 1] }}
>
  Hello
</motion.div>
```

## 滾動觸發（whileInView）

```tsx
<motion.div
  initial={{ opacity: 0, y: 40 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true, margin: '-100px' }}
  transition={{ duration: 0.6 }}
>
  滾到我才出現
</motion.div>
```

## Staggered Reveal

```tsx
const container = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { staggerChildren: 0.1, delayChildren: 0.2 },
  },
};

const item = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0, transition: { duration: 0.5 } },
};

<motion.ul variants={container} initial="hidden" animate="visible">
  {items.map((i) => <motion.li key={i.id} variants={item}>{i.name}</motion.li>)}
</motion.ul>
```

## Layout Animation（FLIP，最強功能）

```tsx
{items.map((i) => (
  <motion.div key={i.id} layout transition={{ type: 'spring', stiffness: 300 }}>
    {i.name}
  </motion.div>
))}
```

reorder / 增減元素自動順滑動畫。

## 共享元素轉場（layoutId）

```tsx
// 縮圖
<motion.div layoutId="hero-image" />

// 大圖（同 layoutId 自動 morph）
<motion.div layoutId="hero-image" />
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| FM1 | 用 `motion/react` 而非舊 `framer-motion` import（2026 後）| 套件改名 |
| FM2 | 大型場景用 `LazyMotion + domAnimation` 減 bundle | 從 ~38KB 降到 ~6KB |
| FM3 | server component 不能用 motion | 需 `'use client'` |
| FM4 | scroll 動畫用 `whileInView` + `once: true` | 不重複觸發 |
| FM5 | 用 `layout` 處理 reorder | 比手寫 transition 順 |
| FM6 | reduced motion 用 `useReducedMotion` hook | 自動關 |
| FM7 | 動畫值用 `MotionValue` 共享 | 不引發 re-render |

## LazyMotion 範例

```tsx
'use client';
import { LazyMotion, domAnimation, m } from 'motion/react';

<LazyMotion features={domAnimation}>
  <m.div animate={{ x: 100 }} />  {/* 用 m 不用 motion */}
</LazyMotion>
```

## Reduced Motion

```tsx
import { useReducedMotion } from 'motion/react';

const reduced = useReducedMotion();
<motion.div animate={{ y: reduced ? 0 : 100 }} transition={{ duration: reduced ? 0 : 0.5 }} />
```

## AnimatePresence（離場動畫）

```tsx
<AnimatePresence mode="wait">
  {isOpen && (
    <motion.div
      key="modal"
      initial={{ opacity: 0, scale: 0.9 }}
      animate={{ opacity: 1, scale: 1 }}
      exit={{ opacity: 0, scale: 0.9 }}
    >
      Modal
    </motion.div>
  )}
</AnimatePresence>
```

## Spring 物理動畫

```tsx
<motion.div
  animate={{ x: 100 }}
  transition={{ type: 'spring', stiffness: 260, damping: 20 }}
/>
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 整站塞滿 motion | 只用在關鍵時刻 |
| 用 framer-motion 做 hover（CSS 即可）| CSS transition |
| 全 import `motion`（38KB） | LazyMotion + domAnimation |
| 不處理 reducedMotion | useReducedMotion |
| AnimatePresence 內 key 不變 | 必設不同 key |
