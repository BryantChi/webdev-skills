---
id: web_form_tanstack
name: web_form_tanstack
description: TanStack Form — 型別最強的 headless 表單庫
---

# TanStack Form

## 為何選 TanStack Form

- **型別推導最完整**：欄位 path、value、validator 都嚴格型別
- Headless：完全不限制 UI（適合 design system）
- 框架無關：React / Vue / Solid / Lit / Angular 都有
- 與 TanStack Query 一脈

## 安裝

```bash
npm i @tanstack/react-form
```

## 基本範例（Zod 驗證）

```tsx
import { useForm } from '@tanstack/react-form';
import { z } from 'zod';

const schema = z.object({
  email: z.string().email('Email 格式錯誤'),
  password: z.string().min(8, '至少 8 字'),
});

export function LoginForm() {
  const form = useForm({
    defaultValues: { email: '', password: '' },
    validators: { onChange: schema },
    onSubmit: async ({ value }) => {
      await login(value);
    },
  });

  return (
    <form onSubmit={(e) => { e.preventDefault(); form.handleSubmit(); }}>
      <form.Field name="email">
        {(field) => (
          <div>
            <label htmlFor={field.name}>Email</label>
            <input
              id={field.name}
              value={field.state.value}
              onBlur={field.handleBlur}
              onChange={(e) => field.handleChange(e.target.value)}
              aria-invalid={field.state.meta.errors.length > 0}
              aria-describedby={field.state.meta.errors[0] ? `${field.name}-err` : undefined}
            />
            {field.state.meta.errors[0] && (
              <p id={`${field.name}-err`} role="alert">{field.state.meta.errors[0]}</p>
            )}
          </div>
        )}
      </form.Field>

      <form.Subscribe selector={(s) => [s.canSubmit, s.isSubmitting]}>
        {([canSubmit, isSubmitting]) => (
          <button type="submit" disabled={!canSubmit || isSubmitting}>
            {isSubmitting ? '送出中' : '登入'}
          </button>
        )}
      </form.Subscribe>
    </form>
  );
}
```

## CRITICAL 規則

| # | Rule |
|---|---|
| TF1 | validator 用 schema（zod/valibot），不要散落 | 集中、可重用 |
| TF2 | 訂閱 state 用 `Subscribe` 限制 re-render | 性能 |
| TF3 | 動態欄位用 `pushValue`/`removeValue` | 內建 |
| TF4 | async 驗證（如 Email 重複）內建 debounce | `validators.onChangeAsyncDebounceMs` |
| TF5 | submit 中 disable | `s.isSubmitting` |

## Async 驗證範例

```tsx
<form.Field
  name="username"
  validators={{
    onChangeAsyncDebounceMs: 500,
    onChangeAsync: async ({ value }) => {
      const res = await fetch(`/api/check-username?u=${value}`);
      const { available } = await res.json();
      return available ? undefined : '此用戶名已被使用';
    },
  }}
>
  {(field) => (...)}
</form.Field>
```

## RHF vs TanStack Form

| 項 | RHF | TanStack Form |
|---|---|---|
| 主流度 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| 型別嚴格度 | 中 | 極強 |
| 框架支援 | 僅 React | 多框架 |
| 生態 | 豐富（shadcn/ui Form） | 較新 |
| 學習曲線 | 中 | 中高 |
| 包大小 | 中 | 中 |

**建議**：
- 已用 React + shadcn → RHF
- 多框架統一 / 需要極強型別 → TanStack Form

## Anti-Pattern

| Bad | Good |
|---|---|
| 訂閱整個 form state | `Subscribe` + selector |
| async 驗每次按鍵 | `onChangeAsyncDebounceMs` |
| 自管 isDirty / isValid | 用內建 `formState` |
