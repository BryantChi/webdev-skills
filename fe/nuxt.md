---
name: Nuxt.js 指南
description: Nuxt.js 3 開發最佳實踐
---

# Nuxt.js 指南

## 專案建立

```bash
npx nuxi@latest init my-app
cd my-app
npm install
npm run dev
```

---

## 專案結構

```
app/
├── app.vue           # 根元件
├── pages/            # 頁面 (自動路由)
├── components/       # 元件 (自動載入)
├── composables/      # Composables (自動載入)
├── layouts/          # 佈局
├── middleware/       # 路由中間件
├── plugins/          # 插件
├── server/           # API 路由
│   └── api/
└── assets/           # 靜態資源
```

---

## 頁面與路由

```vue
<!-- pages/index.vue -->
<template>
  <div>
    <h1>首頁</h1>
  </div>
</template>

<!-- pages/posts/[id].vue -->
<script setup>
const route = useRoute()
const { data: post } = await useFetch(`/api/posts/${route.params.id}`)
</script>

<template>
  <article>
    <h1>{{ post.title }}</h1>
  </article>
</template>
```

---

## 資料獲取

```vue
<script setup>
// 伺服器端獲取
const { data, pending, error, refresh } = await useFetch('/api/users')

// 僅客戶端
const { data } = await useFetch('/api/users', {
  server: false
})

// 快取控制
const { data } = await useFetch('/api/users', {
  key: 'users',
  getCachedData(key) {
    return nuxtApp.payload.data[key]
  }
})
</script>
```

---

## Server API

```typescript
// server/api/users.ts
export default defineEventHandler(async (event) => {
  const users = await db.users.findMany()
  return users
})

// server/api/users/[id].ts
export default defineEventHandler(async (event) => {
  const id = event.context.params.id
  return await db.users.findUnique({ where: { id } })
})
```

---

## 最佳實踐

1. **使用 Auto-import**
2. **Composables 抽取邏輯**
3. **Server API 分離後端**
4. **使用 useFetch 獲取資料**
5. **Middleware 統一處理權限**
6. **Layouts 管理佈局**
