---
id: web_form_rhf
name: web_form_rhf
description: React Hook Form + Zod 完整實作
---

# React Hook Form + Zod

## 為何選 RHF

- 非受控（uncontrolled）→ 性能最佳
- 與 Zod schema 完美整合
- TS 型別自動推導
- 生態最豐富（含 shadcn/ui Form）

## 安裝

```bash
npm i react-hook-form zod @hookform/resolvers
```

## 完整範例

```tsx
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const schema = z.object({
  email: z.string().email('Email 格式錯誤'),
  password: z.string().min(8, '密碼至少 8 字'),
  age: z.coerce.number().int().min(18, '需年滿 18'),
  terms: z.literal(true, { errorMap: () => ({ message: '需同意條款' }) }),
});

type FormData = z.infer<typeof schema>;

export function SignUpForm() {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
    setError,
    setFocus,
  } = useForm<FormData>({
    resolver: zodResolver(schema),
    mode: 'onBlur',
    defaultValues: { email: '', password: '', age: 18, terms: false },
  });

  const onSubmit = async (data: FormData) => {
    try {
      const res = await fetch('/api/signup', {
        method: 'POST', body: JSON.stringify(data),
      });
      if (!res.ok) {
        const err = await res.json();
        if (err.field) {
          setError(err.field, { message: err.message });
          setFocus(err.field);
        }
        return;
      }
      // 成功
    } catch (e) {
      setError('root', { message: '系統錯誤，請稍後再試' });
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} noValidate>
      <div>
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          aria-invalid={!!errors.email}
          aria-describedby={errors.email ? 'email-err' : undefined}
          {...register('email')}
        />
        {errors.email && <p id="email-err" role="alert">{errors.email.message}</p>}
      </div>

      <div>
        <label htmlFor="password">密碼</label>
        <input
          id="password"
          type="password"
          aria-invalid={!!errors.password}
          aria-describedby={errors.password ? 'pw-err' : undefined}
          {...register('password')}
        />
        {errors.password && <p id="pw-err" role="alert">{errors.password.message}</p>}
      </div>

      {errors.root && <p role="alert">{errors.root.message}</p>}

      <button type="submit" disabled={isSubmitting} aria-busy={isSubmitting}>
        {isSubmitting ? '送出中...' : '註冊'}
      </button>
    </form>
  );
}
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| RHF1 | schema 與 form 共用型別 | 兩處重複定義 | `z.infer<typeof schema>` |
| RHF2 | 用 `mode: 'onBlur'` 或 `'onChange'`（不要全 onSubmit） | 太晚提示 | onBlur 較佳體驗 |
| RHF3 | 動態欄位用 `useFieldArray` | 自管陣列 | RHF 內建 |
| RHF4 | 跨欄位驗證用 Zod refine / superRefine | 自寫 | Zod 內建 |
| RHF5 | submit 中 disable 按鈕 | 重複送 | `disabled={isSubmitting}` |
| RHF6 | server 錯誤回 → `setError` + `setFocus` | 只 console | 設到欄位 |
| RHF7 | 表單元件用 `Controller` 包非標準 input | 自寫 ref | Controller |

## Controller（自訂 input）

```tsx
import { Controller } from 'react-hook-form';

<Controller
  name="country"
  control={control}
  render={({ field, fieldState }) => (
    <CustomSelect
      value={field.value}
      onChange={field.onChange}
      onBlur={field.onBlur}
      error={fieldState.error?.message}
    />
  )}
/>
```

## 動態欄位陣列

```tsx
import { useFieldArray } from 'react-hook-form';

const { fields, append, remove } = useFieldArray({ control, name: 'phones' });

return fields.map((f, i) => (
  <div key={f.id}>
    <input {...register(`phones.${i}.number`)} />
    <button type="button" onClick={() => remove(i)}>刪</button>
  </div>
));
```

## 跨欄位驗證

```ts
const schema = z.object({
  password: z.string().min(8),
  confirm: z.string(),
}).refine((d) => d.password === d.confirm, {
  message: '密碼不一致',
  path: ['confirm'],
});
```

## shadcn/ui Form 整合

```tsx
import { Form, FormField, FormItem, FormLabel, FormControl, FormMessage } from '@/components/ui/form';
import { Input } from '@/components/ui/input';

<Form {...form}>
  <form onSubmit={form.handleSubmit(onSubmit)}>
    <FormField
      control={form.control}
      name="email"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Email</FormLabel>
          <FormControl><Input {...field} /></FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  </form>
</Form>
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 自己 useState + onChange | RHF register |
| 不 await 也不 disable 按鈕 | `await + disabled` |
| 錯誤訊息不 a11y 關聯 | `aria-describedby` + `role="alert"` |
| 只 onSubmit 才驗 | `mode: 'onBlur'` |
| 重複定義 type 與 schema | `z.infer` 推導 |
