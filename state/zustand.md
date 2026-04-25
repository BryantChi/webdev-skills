---
id: web_state_zustand
name: web_state_zustand
description: Zustand 完整指南（推薦中小型 React 應用）
---

# Zustand

## 為何選 Zustand

- 比 Redux 少 90% boilerplate
- 比 Context 不會無謂 re-render
- TypeScript 型別自然
- 套件 < 1KB

## 安裝

```bash
npm i zustand
```

## 基本範例

```ts
// stores/cart.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

type CartItem = { id: string; name: string; qty: number };
type CartState = {
  items: CartItem[];
  add: (item: CartItem) => void;
  remove: (id: string) => void;
  clear: () => void;
  total: () => number;
};

export const useCart = create<CartState>()(
  persist(
    (set, get) => ({
      items: [],
      add: (item) =>
        set((s) => {
          const existing = s.items.find((i) => i.id === item.id);
          if (existing) {
            return { items: s.items.map((i) => i.id === item.id ? { ...i, qty: i.qty + item.qty } : i) };
          }
          return { items: [...s.items, item] };
        }),
      remove: (id) => set((s) => ({ items: s.items.filter((i) => i.id !== id) })),
      clear: () => set({ items: [] }),
      total: () => get().items.reduce((sum, i) => sum + i.qty, 0),
    }),
    { name: 'cart-storage' }
  )
);
```

## 使用（避免 re-render）

```tsx
// ❌ 會在任何 cart 變動 re-render
const cart = useCart();

// ✅ 只挑需要的 slice
const itemCount = useCart((s) => s.items.length);
const add = useCart((s) => s.add);
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| Z1 | 用 selector，不 destructure | `const { x, y } = useStore()` | `useStore((s) => s.x)` |
| Z2 | actions 放 store 內 | 元件內寫 mutate logic | `set((s) => ...)` 在 store |
| Z3 | 不要把 server 資料放 Zustand | `useStore.users` | TanStack Query |
| Z4 | TS：用 `create<T>()(...)` 兩段式呼叫 | 單段 | 兩段（middleware 友善）|
| Z5 | persist 只存必要欄位 | 全存 | `partialize: (s) => ({ items: s.items })` |

## Slices 模式（store 拆分）

```ts
import { create, StateCreator } from 'zustand';

type CartSlice = { items: CartItem[]; add: (i: CartItem) => void };
type UISlice = { sidebarOpen: boolean; toggleSidebar: () => void };

const cartSlice: StateCreator<CartSlice & UISlice, [], [], CartSlice> = (set) => ({
  items: [], add: (i) => set((s) => ({ items: [...s.items, i] })),
});

const uiSlice: StateCreator<CartSlice & UISlice, [], [], UISlice> = (set) => ({
  sidebarOpen: false, toggleSidebar: () => set((s) => ({ sidebarOpen: !s.sidebarOpen })),
});

export const useStore = create<CartSlice & UISlice>()((...a) => ({
  ...cartSlice(...a),
  ...uiSlice(...a),
}));
```

## SSR / Next App Router

```ts
// stores/createStore.ts
import { createStore } from 'zustand/vanilla';
export const createCartStore = (init: Partial<CartState> = {}) =>
  createStore<CartState>()((set) => ({ items: [], ...init, /* ... */ }));
```

```tsx
// providers/CartProvider.tsx
'use client';
import { createContext, useContext, useRef } from 'react';
import { useStore } from 'zustand';

const Ctx = createContext<ReturnType<typeof createCartStore> | null>(null);

export function CartProvider({ children }) {
  const ref = useRef(createCartStore());
  return <Ctx.Provider value={ref.current}>{children}</Ctx.Provider>;
}

export const useCart = <T,>(sel: (s: CartState) => T) => {
  const store = useContext(Ctx)!;
  return useStore(store, sel);
};
```

## 與 Immer 結合

```ts
import { immer } from 'zustand/middleware/immer';

create<State>()(immer((set) => ({
  items: [],
  add: (item) => set((s) => { s.items.push(item); }),  // 直接 mutate
})));
```

## DevTools

```ts
import { devtools } from 'zustand/middleware';
create<State>()(devtools((set) => ({...}), { name: 'CartStore' }));
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 整個 store 塞一支大 object | 拆 slices |
| persist 全部資料 | 用 `partialize` |
| 用 Zustand 取代 Server state | TanStack Query |
| store action 內 fetch | 抽出去（thunk-like 在元件）或用 RTK Query |
