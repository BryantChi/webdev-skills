---
id: web_state_redux_toolkit
name: web_state_redux_toolkit
description: Redux Toolkit (RTK) — 大型/嚴謹 React 應用的標準
---

# Redux Toolkit (RTK)

## 何時選 RTK

- **多人團隊、需要嚴格約束**：明確的 action / reducer / selector 邊界
- **複雜 state 互動**：跨多個 slice 的衍生計算
- **時間旅行 debug 是剛需**：Redux DevTools
- **已有 Redux 程式碼**：以 RTK 重構

> 中小專案請優先用 [Zustand](zustand.md) + [TanStack Query](tanstack-query.md)，**不要無腦上 Redux**。

## 安裝

```bash
npm i @reduxjs/toolkit react-redux
```

## Store 設定

```ts
// store/index.ts
import { configureStore } from '@reduxjs/toolkit';
import { setupListeners } from '@reduxjs/toolkit/query';
import cart from './cartSlice';
import { api } from './apiSlice';

export const store = configureStore({
  reducer: {
    cart,
    [api.reducerPath]: api.reducer,
  },
  middleware: (gDM) => gDM().concat(api.middleware),
});

setupListeners(store.dispatch);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

## Slice（取代手寫 action + reducer）

```ts
// store/cartSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

type CartItem = { id: string; qty: number };
type CartState = { items: CartItem[] };

const slice = createSlice({
  name: 'cart',
  initialState: { items: [] } as CartState,
  reducers: {
    add: (state, action: PayloadAction<CartItem>) => {
      const existing = state.items.find((i) => i.id === action.payload.id);
      if (existing) existing.qty += action.payload.qty;
      else state.items.push(action.payload);
    },
    remove: (state, action: PayloadAction<string>) => {
      state.items = state.items.filter((i) => i.id !== action.payload);
    },
  },
});

export const { add, remove } = slice.actions;
export default slice.reducer;
```

> RTK 內建 Immer，可直接「mutate」state。

## RTK Query（取代 TanStack Query）

```ts
// store/apiSlice.ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const api = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  tagTypes: ['User', 'Post'],
  endpoints: (build) => ({
    getUsers: build.query<User[], { role?: string }>({
      query: ({ role } = {}) => `/users?role=${role ?? ''}`,
      providesTags: (result) =>
        result ? [...result.map(({ id }) => ({ type: 'User' as const, id })), 'User'] : ['User'],
    }),
    addUser: build.mutation<User, Partial<User>>({
      query: (body) => ({ url: '/users', method: 'POST', body }),
      invalidatesTags: ['User'],
    }),
  }),
});

export const { useGetUsersQuery, useAddUserMutation } = api;
```

```tsx
// 使用
const { data, isLoading } = useGetUsersQuery({ role: 'admin' });
const [addUser] = useAddUserMutation();
```

## 型別化 hook

```ts
// hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| RT1 | 用 RTK Query 取代手寫 fetch | 統一 cache + invalidation |
| RT2 | Slice 內 mutate（Immer 處理） | 不用回傳新 object |
| RT3 | 用 `tagTypes` + `providesTags/invalidatesTags` 管 cache | 比手動 invalidate 嚴謹 |
| RT4 | selector 用 `createSelector` memoize | 避免重算 |
| RT5 | 不在 reducer 內 fetch | 用 thunk / RTK Query |
| RT6 | 用 typed `useAppDispatch/Selector` | 型別安全 |

## RTK 與 Zustand 何時選哪個

| 情境 | 選 |
|------|------|
| 全公司多 React 專案要一致 | RTK |
| 需 Redux DevTools 時間旅行 | RTK |
| 5 人以下小團隊、輕量 | Zustand |
| 已有 Redux 包袱 | RTK 重構 |
| 純客戶端 SPA、簡單 | Zustand |

## Anti-Pattern

| Bad | Good |
|---|---|
| 手寫 `createAction` + `createReducer` | `createSlice` |
| RTK 配 axios + 自管 cache | RTK Query |
| Selector 直接做計算 | `createSelector` 記憶 |
| 一個 mega slice | 拆多個 feature slice |
