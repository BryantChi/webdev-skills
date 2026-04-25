---
id: web_testing_visual_regression
name: web_testing_visual_regression
description: 視覺回歸測試（Playwright screenshots / Percy / Chromatic）
---

# 視覺回歸測試

## Playwright 內建（推薦起步）

```ts
test('首頁視覺一致', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveScreenshot('home.png', { maxDiffPixels: 100 });
});
```

第一次跑：`npx playwright test --update-snapshots`，後續若像素差超過閥值則失敗。

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| V1 | 鎖定字型、動畫、隨機資料 | 避免假陽性 |
| V2 | 在固定 viewport 截圖 | mobile / desktop 各一組 |
| V3 | 等待網路 idle 再截 | `waitForLoadState('networkidle')` |
| V4 | snapshot 提交進 git，但 .gitignore 跨平台差異 | 用 docker / CI 同 OS |

## 穩定截圖的設定

```ts
test.use({
  viewport: { width: 1280, height: 800 },
});

test.beforeEach(async ({ page }) => {
  // 關閉動畫、固定時間
  await page.addStyleTag({
    content: `*, *::before, *::after { animation: none !important; transition: none !important; }`,
  });
});
```

## 平台選項

| 服務 | 適用 | 計費 |
|------|------|------|
| Playwright 內建 | 起步、開源 | 免費 |
| Percy | 專業 review UI | 付費 |
| Chromatic | Storybook 整合 | 付費（個人免費） |
| Argos CI | git 整合好 | 開源 + SaaS |

## 與 Storybook 整合

每個 story 自動視覺測試：

```ts
// .storybook/test-runner.ts
import { toHaveScreenshot } from '@playwright/test';
// 用 storybook test-runner + axe + screenshot
```

## 何時不用視覺回歸

- 內容完全動態的頁面（如 dashboard 即時資料）
- 改版頻繁的早期專案（噪音多）

對這類頁面只測「有顯示某結構」，不測像素。
