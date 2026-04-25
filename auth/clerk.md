---
id: web_auth_clerk
name: web_auth_clerk
description: Clerk — SaaS 多租戶完整 UI + 認證解決方案
---

# Clerk

## 何時選 Clerk

- **不想自管認證 UI / DB**：登入註冊頁、忘記密碼、Email 驗證內建
- **多租戶 SaaS（Organizations）**：Clerk 內建 org / member / invite
- **B2B / 需快速上線**：Stripe-like 的 Auth 體驗
- **全球部署**：Clerk 全球邊緣

## 限制

- **付費**（免費 10k MAU 以內）
- 認證 UI 客製需在限度內
- 廠商鎖定

## 安裝（Next 14/15）

```bash
npm i @clerk/nextjs
```

```ts
// middleware.ts
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server';

const isProtected = createRouteMatcher(['/dashboard(.*)', '/settings(.*)']);

export default clerkMiddleware(async (auth, req) => {
  if (isProtected(req)) await auth.protect();
});

export const config = { matcher: ['/((?!.*\\..*|_next).*)', '/', '/(api|trpc)(.*)'] };
```

```tsx
// app/layout.tsx
import { ClerkProvider } from '@clerk/nextjs';

export default function Layout({ children }) {
  return (
    <ClerkProvider>
      <html><body>{children}</body></html>
    </ClerkProvider>
  );
}
```

## 登入 / 註冊頁

```tsx
// app/sign-in/[[...sign-in]]/page.tsx
import { SignIn } from '@clerk/nextjs';
export default () => <SignIn />;
```

```tsx
// app/sign-up/[[...sign-up]]/page.tsx
import { SignUp } from '@clerk/nextjs';
export default () => <SignUp />;
```

## 取得使用者

```tsx
// Server component
import { auth, currentUser } from '@clerk/nextjs/server';

const { userId, sessionClaims } = await auth();
const user = await currentUser();
```

```tsx
// Client component
'use client';
import { useUser, useAuth } from '@clerk/nextjs';

const { user } = useUser();
const { isSignedIn, signOut } = useAuth();
```

```ts
// Server action
'use server';
import { auth } from '@clerk/nextjs/server';

export async function createPost(data: FormData) {
  const { userId } = await auth.protect();
  // userId 已驗
}
```

## Organizations（多租戶）

```tsx
import { OrganizationSwitcher, OrganizationProfile } from '@clerk/nextjs';

<OrganizationSwitcher />
<OrganizationProfile />
```

```ts
// 取目前 org
import { auth } from '@clerk/nextjs/server';
const { orgId, orgRole } = await auth();
```

```ts
// 限定 org admin
await auth.protect({ role: 'org:admin' });
```

## 自訂 publicMetadata（角色 / 計費）

```ts
// 寫入（後端）
import { clerkClient } from '@clerk/nextjs/server';
await clerkClient.users.updateUser(userId, {
  publicMetadata: { plan: 'pro', role: 'editor' },
});
```

```ts
// 讀取（前後端皆可）
const { sessionClaims } = await auth();
const plan = sessionClaims?.metadata?.plan;
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| CL1 | middleware 用 `clerkMiddleware` 集中保護 | 避免每頁判 |
| CL2 | server action 用 `auth.protect()` | 內含驗證 + 角色 |
| CL3 | 不在 client 信任 metadata | 寫入只能 server |
| CL4 | webhook 驗 svix signature | 防偽造 |
| CL5 | publishableKey 公開、secretKey 嚴守 | 命名分清 |
| CL6 | 用 `<Protect>` 元件條件渲染 | 不要 client 自判 |

## Webhook（同步到自家 DB）

```ts
// app/api/clerk/webhook/route.ts
import { Webhook } from 'svix';
import { headers } from 'next/headers';

export async function POST(req: Request) {
  const payload = await req.text();
  const wh = new Webhook(process.env.CLERK_WEBHOOK_SECRET!);
  const h = headers();
  const evt = wh.verify(payload, {
    'svix-id': h.get('svix-id')!,
    'svix-timestamp': h.get('svix-timestamp')!,
    'svix-signature': h.get('svix-signature')!,
  }) as any;

  if (evt.type === 'user.created') await db.users.create({ data: { clerkId: evt.data.id, email: evt.data.email_addresses[0].email_address } });
  if (evt.type === 'user.deleted') await db.users.delete({ where: { clerkId: evt.data.id } });
  return new Response('ok');
}
```

## Clerk vs Auth.js

| 項 | Clerk | Auth.js |
|---|---|---|
| UI 內建 | ✅ 完整 | ❌ 自寫 |
| 多租戶 (Orgs) | ✅ 內建 | ❌ 自實作 |
| 計費 | 付費 | 免費 |
| 客製空間 | 受限 | 無限 |
| 廠商鎖定 | 高 | 低 |
| 上線速度 | 1 天 | 3-7 天 |

## Anti-Pattern

| Bad | Good |
|---|---|
| 把 publicMetadata 當機密 | 機密用 privateMetadata |
| webhook 不驗 signature | svix verify |
| 在 client 自判路由保護 | middleware 處理 |
| 不同步到自家 DB | webhook 同步基礎欄位 |
