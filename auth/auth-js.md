---
id: web_auth_authjs
name: web_auth_authjs
description: Auth.js v5（NextAuth）完整實作 — Next App Router 主流方案
---

# Auth.js v5（NextAuth）

## 為何選 Auth.js

- Next App Router 原生支援
- 80+ OAuth providers 開箱
- 多種 session 策略（database / JWT）
- 與 RSC、middleware、server actions 整合好

## 安裝

```bash
npm i next-auth@beta
```

## 設定

```ts
// auth.ts
import NextAuth from 'next-auth';
import GitHub from 'next-auth/providers/github';
import Google from 'next-auth/providers/google';
import Credentials from 'next-auth/providers/credentials';
import { z } from 'zod';
import bcrypt from 'bcryptjs';
import { db } from './db';

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [
    GitHub,
    Google,
    Credentials({
      credentials: { email: {}, password: {} },
      async authorize(creds) {
        const parsed = z.object({
          email: z.string().email(),
          password: z.string().min(8),
        }).safeParse(creds);
        if (!parsed.success) return null;

        const user = await db.users.findUnique({ where: { email: parsed.data.email } });
        if (!user) return null;
        const ok = await bcrypt.compare(parsed.data.password, user.passwordHash);
        if (!ok) return null;
        return { id: user.id, email: user.email, name: user.name };
      },
    }),
  ],
  pages: { signIn: '/login' },
  session: { strategy: 'jwt', maxAge: 60 * 60 * 24 * 7 },
  callbacks: {
    async jwt({ token, user }) {
      if (user) token.role = user.role;
      return token;
    },
    async session({ session, token }) {
      session.user.role = token.role as string;
      return session;
    },
    authorized({ auth, request: { nextUrl } }) {
      const isLoggedIn = !!auth?.user;
      const isProtected = nextUrl.pathname.startsWith('/dashboard');
      if (isProtected) return isLoggedIn;
      return true;
    },
  },
  trustHost: true,
});
```

```ts
// app/api/auth/[...nextauth]/route.ts
export { GET, POST } from '@/auth';
```

```ts
// middleware.ts
export { auth as middleware } from '@/auth';
export const config = { matcher: ['/((?!api|_next/static|_next/image|favicon.ico).*)'] };
```

## 在元件 / route handler 取 session

```tsx
// Server component
import { auth } from '@/auth';

export default async function Page() {
  const session = await auth();
  if (!session?.user) redirect('/login');
  return <h1>Hello {session.user.name}</h1>;
}
```

```tsx
// Client component
'use client';
import { useSession } from 'next-auth/react';

const { data: session } = useSession();
```

```ts
// Server action
'use server';
import { auth } from '@/auth';

export async function createPost(formData: FormData) {
  const session = await auth();
  if (!session?.user) throw new Error('Unauthorized');
  // ...
}
```

## 登入 / 登出

```tsx
'use client';
import { signIn, signOut } from 'next-auth/react';

<button onClick={() => signIn('github')}>GitHub 登入</button>
<button onClick={() => signOut({ callbackUrl: '/' })}>登出</button>
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| AJ1 | session strategy 選 jwt 或 database | jwt 快、database 可即時 invalidate |
| AJ2 | 用 `authorized` callback 集中保護路由 | 不用每頁判斷 |
| AJ3 | 在 jwt callback 把 role / 自訂欄位塞進 token | 後續取用方便 |
| AJ4 | Credentials provider 必驗 schema | 不信 client |
| AJ5 | `AUTH_SECRET` env 設妥（generate by `npx auth secret`） | 安全基礎 |
| AJ6 | OAuth callback URL 設於 provider 後台 | 避免 token 漏 |
| AJ7 | 重要操作前 re-auth | 防 session hijack |

## RBAC 整合範例

```ts
// lib/rbac.ts
export async function requireRole(role: 'admin' | 'editor') {
  const session = await auth();
  if (!session?.user) throw new Error('Unauthorized');
  if (session.user.role !== role) throw new Error('Forbidden');
  return session.user;
}
```

```ts
// 使用
'use server';
export async function deletePost(id: string) {
  const user = await requireRole('admin');
  await db.posts.delete({ where: { id } });
}
```

## 多 Provider 與帳號合併

```ts
// auth.ts
callbacks: {
  async signIn({ user, account, profile }) {
    if (account?.provider === 'github') {
      // 檢查是否已有 email 對應的帳號
      const existing = await db.users.findUnique({ where: { email: user.email! } });
      if (existing) {
        // 自動連結
        await db.accounts.create({ data: { userId: existing.id, ... } });
      }
    }
    return true;
  },
}
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 在 client 直接讀 token | 用 `auth()` server side |
| middleware 內 fetch DB | middleware 只做 auth check |
| Credentials provider 不驗 schema | Zod parse |
| AUTH_SECRET 弱 / 未設 | `npx auth secret` 產生 |
| 把 jwt strategy 用於需即時禁用的場景 | 改 database strategy |
