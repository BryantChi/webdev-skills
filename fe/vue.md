---
name: Vue 指南
description: Vue 3 Composition API 開發最佳實踐
---

# 💚 Vue 指南

## 專案建立

```bash
# 使用官方腳手架
npm create vue@latest

# 使用 Vite
npm create vite@latest my-app -- --template vue-ts
```

---

## 專案結構

```
src/
├── components/
│   ├── ui/           # 基礎 UI 元件
│   └── common/       # 共用元件
├── composables/      # Composition 函式
├── views/            # 頁面
├── stores/           # Pinia 狀態管理
├── services/         # API 服務
├── types/            # TypeScript 型別
├── router/           # 路由
└── App.vue
```

---

## 元件模式

### Script Setup (推薦)

```vue
<script setup lang="ts">
interface Props {
  name: string;
  email: string;
  avatar?: string;
}

const props = defineProps<Props>();

const emit = defineEmits<{
  (e: 'click', id: number): void;
}>();
</script>

<template>
  <div class="user-card" @click="emit('click', 1)">
    <img v-if="avatar" :src="avatar" :alt="name" />
    <h3>{{ name }}</h3>
    <p>{{ email }}</p>
  </div>
</template>

<style scoped>
.user-card {
  padding: 1rem;
  border-radius: 8px;
}
</style>
```

---

## Composables

```ts
// composables/useUser.ts
import { ref, onMounted } from 'vue';

export function useUser(userId: string) {
  const user = ref(null);
  const loading = ref(true);
  const error = ref(null);

  onMounted(async () => {
    try {
      user.value = await fetchUser(userId);
    } catch (e) {
      error.value = e;
    } finally {
      loading.value = false;
    }
  });

  return { user, loading, error };
}
```

---

## 狀態管理 (Pinia)

```ts
// stores/user.ts
import { defineStore } from 'pinia';

export const useUserStore = defineStore('user', {
  state: () => ({
    user: null as User | null,
  }),
  
  getters: {
    isLoggedIn: (state) => !!state.user,
  },
  
  actions: {
    async login(credentials: Credentials) {
      this.user = await authService.login(credentials);
    },
    logout() {
      this.user = null;
    },
  },
});
```

---

## 最佳實踐

1. **使用 Script Setup + TypeScript**
2. **Composables 抽取可重用邏輯**
3. **Pinia 管理全域狀態**
4. **scoped CSS 避免污染**
5. **v-memo 優化大量列表**
6. **使用 shallowRef 優化大物件**
