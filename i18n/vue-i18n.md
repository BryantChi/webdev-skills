---
id: web_i18n_vue
name: web_i18n_vue
description: Vue 3 / Nuxt 3 i18n 實作（vue-i18n + @nuxtjs/i18n）
---

# vue-i18n / @nuxtjs/i18n

## Vue 3（純前端）

### 安裝

```bash
npm i vue-i18n@9
```

### 設定

```ts
// src/i18n/index.ts
import { createI18n } from 'vue-i18n';
import zhTW from './locales/zh-TW.json';
import en from './locales/en.json';

export const i18n = createI18n({
  legacy: false,
  locale: 'zh-TW',
  fallbackLocale: 'en',
  messages: { 'zh-TW': zhTW, en },
  datetimeFormats: {
    'zh-TW': { short: { year: 'numeric', month: '2-digit', day: '2-digit' } },
  },
});
```

```ts
// main.ts
import { i18n } from './i18n';
app.use(i18n);
```

### 元件使用

```vue
<script setup>
import { useI18n } from 'vue-i18n';
const { t, d, n, locale } = useI18n();
</script>

<template>
  <h1>{{ t('home.title') }}</h1>
  <p>{{ t('items', { count }, count) }}</p>
  <p>{{ d(new Date(), 'short') }}</p>
  <i18n-t keypath="welcome" tag="p">
    <template #name><strong>{{ user.name }}</strong></template>
  </i18n-t>
</template>
```

### 訊息檔

```json
{
  "home": { "title": "首頁" },
  "welcome": "你好，{name}",
  "items": "沒有項目 | {n} 個項目"
}
```

## Nuxt 3（推薦）

### 安裝

```bash
npm i @nuxtjs/i18n
```

### `nuxt.config.ts`

```ts
export default defineNuxtConfig({
  modules: ['@nuxtjs/i18n'],
  i18n: {
    locales: [
      { code: 'zh-TW', iso: 'zh-Hant-TW', name: '繁體中文', file: 'zh-TW.json' },
      { code: 'en', iso: 'en-US', name: 'English', file: 'en.json' },
    ],
    defaultLocale: 'zh-TW',
    strategy: 'prefix',  // 預設語言也加前綴
    langDir: 'locales/',
    detectBrowserLanguage: {
      useCookie: true,
      cookieKey: 'i18n_redirected',
      redirectOn: 'root',
    },
  },
});
```

### 在頁面 / 元件

```vue
<script setup>
const { t, locale, locales, setLocale } = useI18n();
</script>

<template>
  <h1>{{ t('home.title') }}</h1>
  <select :model-value="locale" @change="setLocale($event.target.value)">
    <option v-for="l in locales" :key="l.code" :value="l.code">{{ l.name }}</option>
  </select>
</template>
```

### SEO（hreflang 自動）

```vue
<script setup>
useHead(useLocaleHead({ addSeoAttributes: true }));
</script>
```

模組會自動產出 `hreflang`、`<html lang>`、canonical。

## CRITICAL 規則

| # | Rule |
|---|---|
| VI1 | `strategy: 'prefix'`（含預設語） |
| VI2 | `useLocaleHead` 處理 SEO，不要手寫 |
| VI3 | 路由用 `<NuxtLink :to="localePath('/about')">` |
| VI4 | 翻譯 key 命名空間化（`home.title` 而非 `homeTitle`） |
| VI5 | 用 `i18n-t`（vue-i18n）/ `<i18n-link>` 處理插入元素 |

## Pinia 整合

不要在 Pinia store 內存翻譯字串。讓 store 存 raw data，元件層再 `t()` 翻譯。
