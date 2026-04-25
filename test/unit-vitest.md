---
id: web_testing_unit_vitest
name: web_testing_unit_vitest
description: Vitest + Testing Library 單元/整合測試指南
---

# Vitest 單元測試

## 安裝

```bash
npm i -D vitest @testing-library/react @testing-library/jest-dom jsdom @testing-library/user-event
# Vue: @testing-library/vue
```

## `vitest.config.ts`

```ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: ['./test/setup.ts'],
    globals: true,
    coverage: { provider: 'v8', thresholds: { lines: 80, branches: 75 } },
  },
});
```

## React 元件範例

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Counter } from './Counter';

test('點擊按鈕計數加一', async () => {
  render(<Counter />);
  const btn = screen.getByRole('button', { name: /增加/ });
  await userEvent.click(btn);
  expect(screen.getByText('1')).toBeInTheDocument();
});
```

## CRITICAL 規則

| # | Rule | 範例 |
|---|---|---|
| U1 | 用 `getByRole` / `getByLabelText`，不要 `querySelector` | 對齊使用者視角 |
| U2 | 用 `userEvent`，不用 `fireEvent` | 模擬真實互動 |
| U3 | 測試行為，不測 state | 不要 `wrapper.state()` |
| U4 | 不 mock 自家元件 | 真組合更貼近實際 |
| U5 | API 用 MSW mock，不要 axios mock | 整合更真 |

## MSW 範例

```ts
// test/mocks/handlers.ts
import { http, HttpResponse } from 'msw';
export const handlers = [
  http.get('/api/users', () => HttpResponse.json([{ id: 1, name: 'Alice' }])),
];
```

```ts
// test/setup.ts
import { server } from './mocks/server';
beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

## 涵蓋什麼

| 該測 | 不該測 |
|---|---|
| 公開 API 行為 | 私有實作細節 |
| 邊界條件、錯誤分支 | getter/setter 直接賦值 |
| 自訂 hook 邏輯 | 第三方 lib |
| 表單驗證規則 | 純樣式 |

## 與 Jest 的差異

Vitest 速度快 3-10 倍，API 99% 相容。新專案直接選 Vitest。
