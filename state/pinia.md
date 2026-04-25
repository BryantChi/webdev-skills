---
id: web_state_pinia
name: web_state_pinia
description: Pinia — Vue 官方推薦的狀態管理（取代 Vuex）
---

# Pinia

## 為何用 Pinia

- Vue 3 官方推薦，取代 Vuex
- 比 Vuex 少 50% 樣板碼，TypeScript 友善
- DevTools 整合佳

## 安裝

```bash
npm i pinia
```

```ts
// main.ts
import { createPinia } from 'pinia';
app.use(createPinia());
```

## Setup Store（推薦，與 Composition API 一致）

```ts
// stores/cart.ts
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';

export const useCart = defineStore('cart', () => {
  const items = ref<CartItem[]>([]);
  const total = computed(() => items.value.reduce((s, i) => s + i.qty * i.price, 0));

  function add(item: CartItem) {
    const existing = items.value.find((i) => i.id === item.id);
    if (existing) existing.qty += item.qty;
    else items.value.push(item);
  }

  function remove(id: string) {
    items.value = items.value.filter((i) => i.id !== id);
  }

  return { items, total, add, remove };
}, {
  persist: true,  // 需 pinia-plugin-persistedstate
});
```

## Options Store（傳統樣式）

```ts
export const useCart = defineStore('cart', {
  state: () => ({ items: [] as CartItem[] }),
  getters: {
    total: (s) => s.items.reduce((sum, i) => sum + i.qty, 0),
  },
  actions: {
    add(item: CartItem) { this.items.push(item); },
  },
});
```

## 元件中使用

```vue
<script setup>
import { storeToRefs } from 'pinia';
import { useCart } from '@/stores/cart';

const cart = useCart();
const { items, total } = storeToRefs(cart);  // 保持響應性
</script>

<template>
  <div>
    <p>共 {{ total }} 元</p>
    <button @click="cart.add({ id: '1', qty: 1, price: 100 })">加入</button>
  </div>
</template>
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| PI1 | 解構保留響應性 | `const { items } = useCart()` | `const { items } = storeToRefs(useCart())` |
| PI2 | actions 直接呼叫 | `useCart().value.add()` | `useCart().add()` |
| PI3 | 不在 setup store 用 `this` | options 才能用 | setup 用 ref/computed |
| PI4 | persist 只存必要欄位 | 全存 | `persist: { paths: ['items'] }` |
| PI5 | 不把 server 資料放 store | useCart.users | TanStack Vue Query |

## 與 TanStack Vue Query 搭配

```ts
// Pinia 管 client state
// TanStack Vue Query 管 server state
import { useQuery } from '@tanstack/vue-query';
const { data } = useQuery({ queryKey: ['users'], queryFn: getUsers });
```

## SSR / Nuxt 3

Nuxt 3 用 `@pinia/nuxt`：

```bash
npm i @pinia/nuxt
```

```ts
// nuxt.config.ts
export default defineNuxtConfig({ modules: ['@pinia/nuxt'] });
```

```ts
// stores 自動匯入，使用 useCart() 即可
```

## Persist（持久化）

```bash
npm i pinia-plugin-persistedstate
```

```ts
import { createPinia } from 'pinia';
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate';
const pinia = createPinia();
pinia.use(piniaPluginPersistedstate);
```

```ts
defineStore('cart', () => { ... }, {
  persist: { storage: localStorage, paths: ['items'] },
});
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 直接解構失去響應 | `storeToRefs` |
| 把整 API response 存 store | TanStack Vue Query |
| 一個大 store | 按 feature 拆 |
| 用 Vuex 4 新專案 | 改用 Pinia |
