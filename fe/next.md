---
name: Next.js 指南
description: Next.js 14+ App Router 開發最佳實踐
---

# ▲ Next.js 指南

## 專案建立

```bash
npx create-next-app@latest my-app --typescript --tailwind --app
```

---

## 專案結構 (App Router)

```
app/
├── layout.tsx        # 根佈局
├── page.tsx          # 首頁
├── globals.css
├── (auth)/           # 路由群組
│   ├── login/
│   └── register/
├── dashboard/
│   ├── layout.tsx
│   └── page.tsx
└── api/              # API 路由
    └── users/
        └── route.ts

components/
├── ui/
└── features/

lib/                  # 工具與服務
```

---

## 渲染模式

### Server Components (預設)

```tsx
// app/users/page.tsx
async function UsersPage() {
  const users = await fetchUsers(); // 伺服器端執行
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### Client Components

```tsx
'use client';

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  );
}
```

---

## API Routes

```ts
// app/api/users/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  const users = await db.users.findMany();
  return NextResponse.json(users);
}

export async function POST(request: Request) {
  const body = await request.json();
  const user = await db.users.create({ data: body });
  return NextResponse.json(user, { status: 201 });
}
```

---

## 資料獲取

### Server Components

```tsx
async function Page() {
  const data = await fetch('https://api.example.com/data', {
    cache: 'force-cache',      // 快取（預設）
    // cache: 'no-store',      // 不快取
    // next: { revalidate: 60 } // ISR
  });
  return <div>{data}</div>;
}
```

### Server Actions

```tsx
async function createUser(formData: FormData) {
  'use server';
  const name = formData.get('name');
  await db.users.create({ data: { name } });
  revalidatePath('/users');
}
```

---

## 最佳實踐

1. **預設使用 Server Components**
2. **僅在需要互動時使用 Client Components**
3. **使用 Server Actions 處理表單**
4. **適當設定 cache 和 revalidate**
5. **使用 loading.tsx 和 error.tsx**
6. **使用 Metadata API 處理 SEO**
