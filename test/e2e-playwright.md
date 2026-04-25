---
id: web_testing_playwright
name: web_testing_playwright
description: Playwright E2E 完整指南，含 Playwright MCP 整合
---

# Playwright E2E

## 安裝與初始化

```bash
npm i -D @playwright/test
npx playwright install
npx playwright install-deps
```

## 基礎範例

```ts
// e2e/login.spec.ts
import { test, expect } from '@playwright/test';

test('使用者可登入並進入儀表板', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('user@test.com');
  await page.getByLabel('密碼').fill('Pa$$w0rd');
  await page.getByRole('button', { name: '登入' }).click();
  await expect(page).toHaveURL('/dashboard');
  await expect(page.getByRole('heading', { name: /歡迎/ })).toBeVisible();
});
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| E1 | Selector 用語意（role/label）優先 | `page.locator('.btn-primary')` | `page.getByRole('button', { name: '送出' })` |
| E2 | 不用固定 sleep | `page.waitForTimeout(2000)` | `page.waitForResponse(/api\/x/)` |
| E3 | 用 `expect.toPass` 處理重試 | 自寫 retry | Playwright 內建 |
| E4 | fixture 隔離 state | 全域變數共用 | `test.extend({ user })` |
| E5 | 並行：每測試獨立資料 | 用同一帳號 | seed 唯一資料 |

## Fixture 模式

```ts
// fixtures.ts
import { test as base } from '@playwright/test';

type Fixtures = { authedPage: Page };

export const test = base.extend<Fixtures>({
  authedPage: async ({ page }, use) => {
    await page.goto('/login');
    // ... 登入流程
    await use(page);
  },
});
```

## Playwright MCP 整合（推薦）

> 由 Anthropic 提供的 MCP server，讓 Claude 可直接操作瀏覽器、抓 accessibility tree。

| 工具 | 用途 |
|------|------|
| `browser_navigate` | 開頁 |
| `browser_snapshot` | 取 a11y 樹（取代截圖，省 token） |
| `browser_click` / `browser_type` | 互動 |
| `browser_evaluate` | 執行 JS |
| `browser_console_messages` | 抓 console error |

**優點**：用 a11y tree 而非截圖，**Token 省 10x**，且更可靠。

## CI 設定

```ts
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';
export default defineConfig({
  testDir: './e2e',
  retries: process.env.CI ? 2 : 0,
  reporter: process.env.CI ? 'github' : 'html',
  use: { baseURL: 'http://localhost:3000', trace: 'on-first-retry' },
  webServer: { command: 'npm run dev', port: 3000, reuseExistingServer: !process.env.CI },
  projects: [
    { name: 'chromium', use: devices['Desktop Chrome'] },
    { name: 'mobile', use: devices['Pixel 5'] },
  ],
});
```

## 測試什麼

**該測**：
- 主要使用者旅程（註冊→使用→結帳）
- 跨頁面 state（購物車）
- 表單驗證錯誤路徑
- 權限/路由保護

**不該測**：
- 純 UI 樣式（用視覺回歸）
- 第三方服務本身
- 已被單元測試覆蓋的邏輯
