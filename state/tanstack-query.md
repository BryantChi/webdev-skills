---
id: web_state_tanstack_query
name: web_state_tanstack_query
description: TanStack Query (React Query) — Server state 的標準解
---

# TanStack Query

## 為何用 TanStack Query

- **取代 useEffect + fetch + useState 的 4 種狀態**：loading / error / data / refetch
- 自動 cache + 去重 + 重試 + 背景更新
- 與 RSC / Server Actions 完美互補

## 安裝

```bash
npm i @tanstack/react-query @tanstack/react-query-devtools
```

## 設定

```tsx
// app/providers.tsx
'use client';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { useState } from 'react';

export function Providers({ children }) {
  const [client] = useState(() => new QueryClient({
    defaultOptions: {
      queries: {
        staleTime: 60 * 1000,        // 1 分鐘內視為新鮮
        gcTime: 5 * 60 * 1000,       // 5 分鐘垃圾回收
        retry: 1,
        refetchOnWindowFocus: true,
      },
    },
  }));
  return <QueryClientProvider client={client}>{children}</QueryClientProvider>;
}
```

## Query 基本範例

```tsx
import { useQuery } from '@tanstack/react-query';

function Users() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users', { role: 'admin' }],
    queryFn: ({ signal }) => fetch('/api/users?role=admin', { signal }).then((r) => r.json()),
  });

  if (isLoading) return <Skeleton />;
  if (error) return <ErrorState error={error} />;
  return <UserList users={data} />;
}
```

## Mutation 範例（含樂觀更新）

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query';

function AddUser() {
  const qc = useQueryClient();
  const m = useMutation({
    mutationFn: (newUser: User) =>
      fetch('/api/users', { method: 'POST', body: JSON.stringify(newUser) }).then((r) => r.json()),
    onMutate: async (newUser) => {
      await qc.cancelQueries({ queryKey: ['users'] });
      const prev = qc.getQueryData(['users']);
      qc.setQueryData(['users'], (old: User[]) => [...old, { ...newUser, id: 'temp' }]);
      return { prev };
    },
    onError: (_err, _new, ctx) => qc.setQueryData(['users'], ctx?.prev),
    onSettled: () => qc.invalidateQueries({ queryKey: ['users'] }),
  });
  return <button onClick={() => m.mutate({ name: 'Bob' })}>Add</button>;
}
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| TQ1 | queryKey 用陣列 + 唯一識別 | `['users']`（同 key 衝突）| `['users', { role, page }]` |
| TQ2 | queryFn 接 `signal` 處理取消 | 不傳 | `fetch(url, { signal })` |
| TQ3 | mutation 用 `invalidateQueries` 而非手動 setData | 易遺漏 | invalidate 全部相關 key |
| TQ4 | 樂觀更新搭配 `onMutate` + rollback | 失敗 UI 卡 | 用 ctx.prev |
| TQ5 | staleTime 視資料性質設 | 全 0 = 一直 fetch | 即時：0；穩定：5min |
| TQ6 | 用 query key factory 集中管理 | 字串散落 | `userKeys.detail(id)` |

## Query Key Factory（推薦）

```ts
export const userKeys = {
  all: ['users'] as const,
  lists: () => [...userKeys.all, 'list'] as const,
  list: (filters: Filters) => [...userKeys.lists(), filters] as const,
  details: () => [...userKeys.all, 'detail'] as const,
  detail: (id: string) => [...userKeys.details(), id] as const,
};

useQuery({ queryKey: userKeys.detail(id), ... });
qc.invalidateQueries({ queryKey: userKeys.lists() });  // 只失效 list
```

## 與 Next App Router 整合

```tsx
// Server Component 預先取
import { dehydrate, HydrationBoundary } from '@tanstack/react-query';

export default async function Page() {
  const qc = new QueryClient();
  await qc.prefetchQuery({ queryKey: ['users'], queryFn: getUsers });
  return (
    <HydrationBoundary state={dehydrate(qc)}>
      <Users />
    </HydrationBoundary>
  );
}
```

## SWR 比較

| 項 | TanStack Query | SWR |
|---|---|---|
| 體積 | 中（~14KB） | 小（~5KB） |
| Mutation | 完整 | 較簡 |
| DevTools | 強大 | 一般 |
| 樂觀更新 | 內建 | 手動 |
| Vue / Solid 支援 | ✅ | ❌（純 React） |

中小型 + React → SWR；複雜 mutation 或要全套 → TanStack Query。

## Anti-Pattern

| Bad | Good |
|---|---|
| `useEffect(() => fetch())` | useQuery |
| 手動管 loading/error state | TanStack 內建 |
| 將 query data 複製到 Zustand | 直接讀 useQuery |
| 同 endpoint 不同 key | 用 query key factory 規範 |
| 不設 staleTime（過度 refetch） | 視資料設 |
