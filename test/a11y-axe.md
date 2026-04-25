---
id: web_testing_a11y_axe
name: web_testing_a11y_axe
description: axe-core 自動化無障礙測試 + WCAG 2.2 AA 規則表
---

# axe-core 無障礙測試

## 整合方式

### Playwright + @axe-core/playwright

```bash
npm i -D @axe-core/playwright
```

```ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('首頁無 a11y 違規', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa', 'wcag22aa'])
    .analyze();
  expect(results.violations).toEqual([]);
});
```

### CI 用 axe-cli

```bash
npx @axe-core/cli http://localhost:3000 --tags wcag2a,wcag2aa
```

## WCAG 2.2 AA 必過清單（CRITICAL）

| # | 條目 | 自動可驗 | 修補 |
|---|------|---------|------|
| A1 | 圖片有 alt | ✅ | `<img alt="說明">` 或 `alt=""`（裝飾） |
| A2 | 表單 label 關聯 | ✅ | `<label for>` 或 `aria-label` |
| A3 | 顏色對比 ≥4.5:1（文字） | ✅ | 改色或加底色 |
| A4 | 顏色對比 ≥3:1（大字/UI） | ✅ | — |
| A5 | 互動元素鍵盤可達 | 部分 | `tabindex="0"` 或改 `<button>` |
| A6 | 焦點可見 | ✅ | `:focus-visible { outline: 2px solid }` |
| A7 | 標題層級正確（h1→h2→h3） | ✅ | 不跳階 |
| A8 | 語言屬性 | ✅ | `<html lang="zh-Hant-TW">` |
| A9 | landmark（header/main/footer/nav） | ✅ | 用語意標籤 |
| A10 | 顏色非唯一資訊 | 手動 | 加 icon / 文字 |

## HIGH 規則（人工驗）

| # | 條目 | 檢查方式 |
|---|------|---------|
| A11 | 閱讀順序合理 | screen reader 順過一次 |
| A12 | 焦點順序合乎邏輯 | tab 走過一遍 |
| A13 | 動畫可暫停（>5s） | 提供 pause 控制 |
| A14 | reduced motion 支援 | `@media (prefers-reduced-motion)` |
| A15 | 表單錯誤訊息與欄位關聯 | `aria-describedby` |
| A16 | 跳到主內容連結 | `<a href="#main">Skip to content</a>` |

## React/Vue 常見修補

```tsx
// ❌ Bad
<div onClick={handleClick}>送出</div>

// ✅ Good
<button onClick={handleClick} type="button">送出</button>
```

```tsx
// ❌ Bad
<img src="/avatar.png" />

// ✅ Good
<img src="/avatar.png" alt="使用者頭像" />
// 或裝飾用：alt=""
```

## 螢幕閱讀器測試

| 平台 | 工具 |
|------|------|
| macOS | VoiceOver（Cmd+F5） |
| Windows | NVDA（免費）/ JAWS |
| 行動 | TalkBack（Android） / VoiceOver（iOS） |

每次 release 至少用一個跑核心流程。

## 整合 Storybook

`@storybook/addon-a11y` 在 Story 旁邊跑 axe，即時看違規。
