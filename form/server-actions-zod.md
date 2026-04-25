---
id: web_form_server_actions
name: web_form_server_actions
description: Next.js 15 Server Actions + zod-form-data 表單實作
---

# Server Actions + Zod

## 核心理念

Next 15 的 server action 把表單從「客戶端 fetch」變成「漸進增強的原生表單」：

- 無 JS 也能送（漸進增強）
- TypeScript 端到端
- 自動序列化 / 反序列化
- 與 React 19 `useActionState` 整合

## 安裝

```bash
npm i zod zod-form-data
```

## 基本架構

```tsx
// app/signup/actions.ts
'use server';

import { z } from 'zod';
import { zfd } from 'zod-form-data';
import { redirect } from 'next/navigation';
import { revalidateTag } from 'next/cache';

const schema = zfd.formData({
  email: zfd.text(z.string().email('Email 格式錯誤')),
  password: zfd.text(z.string().min(8, '至少 8 字')),
  age: zfd.numeric(z.number().int().min(18, '需年滿 18')),
  terms: zfd.checkbox(),
});

export type SignupState = {
  ok: boolean;
  errors?: Record<string, string[]>;
  message?: string;
};

export async function signupAction(prev: SignupState, formData: FormData): Promise<SignupState> {
  const parsed = schema.safeParse(formData);
  if (!parsed.success) {
    return { ok: false, errors: parsed.error.flatten().fieldErrors };
  }

  try {
    await db.users.create({ data: parsed.data });
  } catch (e: any) {
    if (e.code === 'P2002') return { ok: false, message: 'Email 已註冊' };
    throw e;
  }

  revalidateTag('users');
  redirect('/welcome');
}
```

## 元件（React 19 useActionState）

```tsx
// app/signup/page.tsx
'use client';

import { useActionState } from 'react';
import { useFormStatus } from 'react-dom';
import { signupAction, type SignupState } from './actions';

const initial: SignupState = { ok: false };

export default function SignupPage() {
  const [state, action] = useActionState(signupAction, initial);

  return (
    <form action={action}>
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" name="email" type="email" required
          aria-invalid={!!state.errors?.email}
          aria-describedby={state.errors?.email ? 'email-err' : undefined}
        />
        {state.errors?.email && <p id="email-err" role="alert">{state.errors.email[0]}</p>}
      </div>

      <div>
        <label htmlFor="password">密碼</label>
        <input id="password" name="password" type="password" required minLength={8} />
        {state.errors?.password && <p role="alert">{state.errors.password[0]}</p>}
      </div>

      <label><input type="checkbox" name="terms" required /> 同意條款</label>

      {state.message && <p role="alert">{state.message}</p>}

      <SubmitButton />
    </form>
  );
}

function SubmitButton() {
  const { pending } = useFormStatus();
  return <button type="submit" disabled={pending} aria-busy={pending}>
    {pending ? '送出中...' : '註冊'}
  </button>;
}
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| SA1 | server action **必驗 input**（Zod） | 不信 client |
| SA2 | 內部驗權限（getCurrentUser） | server action 是公開 endpoint |
| SA3 | 用 `useActionState` 處理錯誤訊息 | 漸進增強友善 |
| SA4 | mutation 後 `revalidateTag` / `revalidatePath` | 對齊 cache |
| SA5 | redirect 在 try 後（不要在 try 內 catch） | redirect 用 throw |
| SA6 | 機密訊息不返回到 client | 用 generic error |
| SA7 | rate limit（中介層或 KV）| 防爆 |

## 與 RHF 並用（複雜驗證）

server action 可與 RHF 互補：client RHF 即時驗、submit 觸發 server action 再驗一次：

```tsx
'use client';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { signupAction } from './actions';

export function SignupForm() {
  const { register, handleSubmit } = useForm({ resolver: zodResolver(clientSchema) });
  return (
    <form onSubmit={handleSubmit(async (data) => {
      const fd = new FormData();
      Object.entries(data).forEach(([k, v]) => fd.append(k, String(v)));
      await signupAction({ ok: false }, fd);
    })}>
      ...
    </form>
  );
}
```

## 進階：Conform 整合（推薦）

[Conform](https://conform.guide) 把 server action + Zod + 漸進增強整合得最好：

```bash
npm i @conform-to/react @conform-to/zod
```

```tsx
import { useForm } from '@conform-to/react';
import { parseWithZod } from '@conform-to/zod';

const [form, fields] = useForm({
  lastResult,
  onValidate: ({ formData }) => parseWithZod(formData, { schema }),
  shouldValidate: 'onBlur',
});
```

## Anti-Pattern

| Bad | Good |
|---|---|
| server action 不驗 input | 必 Zod 驗 |
| revalidatePath 全站 | 用 tag 精準失效 |
| client throw error 顯示在 client | server action 回 state 物件 |
| 在 server action 內外混用 redirect | 一致風格 |
| 把整個 try-catch 吞 | 區分 expected/unexpected |
