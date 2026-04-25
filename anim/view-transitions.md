---
id: web_animation_view_transitions
name: web_animation_view_transitions
description: View Transitions API — 瀏覽器原生路由轉場（2026 普及）
---

# View Transitions API

## 為何重要

- **瀏覽器原生**（Chrome 111+、Safari 18+、Firefox 即將）
- 同元件跨路由共享動畫（無需 layoutId）
- 體積為零、性能優異
- 自動處理 fade、可客製 morph

## 兩個層次

| 層次 | API | 適用 |
|------|-----|------|
| Same-document | `document.startViewTransition()` | SPA 內部切換 |
| Cross-document | CSS `@view-transition` + `view-transition-name` | 跨 HTML 文件（傳統 server render） |

## SPA：startViewTransition

```ts
// 基本：包住會改 DOM 的動作
document.startViewTransition(() => {
  // DOM 變動
  setActiveTab('overview');
});
```

```css
/* 客製動畫 */
::view-transition-old(root) {
  animation: 0.3s fade-out;
}
::view-transition-new(root) {
  animation: 0.3s fade-in;
}
```

## Next.js App Router 整合

```tsx
'use client';
import { useTransitionRouter } from 'next-view-transitions';
// 或直接：
import { unstable_ViewTransition as ViewTransition } from 'react';

const router = useTransitionRouter();

<button onClick={() => router.push('/about', { onTransitionReady: slideAnim })}>
  About
</button>
```

```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(10px); }
}
::view-transition-old(root) { animation: 0.2s fade-out ease; }
::view-transition-new(root) { animation: 0.3s fade-in ease; }
```

## 共享元素（最強用途）

```tsx
// 列表頁
<img src={post.cover} style={{ viewTransitionName: `cover-${post.id}` }} />

// 詳情頁（同 viewTransitionName）
<img src={post.cover} style={{ viewTransitionName: `cover-${post.id}` }} />
```

切換時兩張圖會自動 morph，無需 framer layoutId。

## Cross-document（MPA 也能用）

```css
/* 兩個 HTML 文件都加 */
@view-transition {
  navigation: auto;
}

/* 共享元素 */
.hero-image {
  view-transition-name: hero;
}
```

點擊連結瀏覽器**會自動處理跨頁轉場**（2026 主流支援）。

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| VT1 | 偵測支援：`if ('startViewTransition' in document)` 才用 | 老瀏覽器降級 |
| VT2 | reduced motion 跳過 | `prefers-reduced-motion: reduce` |
| VT3 | `view-transition-name` 必唯一 | 重複會報錯 |
| VT4 | 動畫期間不要 fetch（會卡）| 先 fetch 後 startViewTransition |
| VT5 | iOS Safari 注意支援版本 | 需 17.4+ |

## 偵測 + 降級

```ts
function transition(callback: () => void) {
  const reduced = matchMedia('(prefers-reduced-motion: reduce)').matches;
  if (!('startViewTransition' in document) || reduced) {
    callback();
    return;
  }
  document.startViewTransition(callback);
}
```

## 比較：何時用何種動畫工具

| 場景 | 工具 |
|------|------|
| SPA 內小切換（tab、模式） | View Transitions（`startViewTransition`） |
| 跨頁面（連結 → 新頁）| View Transitions cross-document |
| React 元件 layout 動畫 | Framer Motion `layout` |
| 複雜時間軸 / scroll 編排 | GSAP |
| 簡單 hover / focus | CSS transition |

## React 19 ViewTransition 元件（實驗性）

```tsx
import { unstable_ViewTransition as ViewTransition } from 'react';

<ViewTransition>
  {tab === 'overview' ? <Overview /> : <Settings />}
</ViewTransition>
```

React 自動 wrap startViewTransition + 處理共享元素。

## 進階：onTransitionReady 同步動畫

```ts
const t = document.startViewTransition(() => updateDOM());
await t.ready;  // 等舊新快照建立
document.documentElement.animate(
  { transform: ['translateX(100%)', 'translateX(0)'] },
  { duration: 300, pseudoElement: '::view-transition-new(root)' }
);
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 不偵測支援直接用 | 用 `'startViewTransition' in document` 守 |
| `view-transition-name` 重複 | UUID 或 ID 化 |
| 動畫中 fetch 大資料 | 先 fetch 後 trigger |
| 自寫複雜 fade（已有原生） | 用 VT API |
| 全頁轉場用 framer-motion 取代 | 改 VT API（效能更好） |
