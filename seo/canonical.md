---
id: technical_seo_canonical
name: technical_seo_canonical
description: Canonical / hreflang / 分頁處理
---

# Canonical / hreflang / Pagination

## Canonical（每頁必設）

```html
<link rel="canonical" href="https://example.com/products/abc">
```

| 規則 | 說明 |
|------|------|
| 每頁自指 canonical | 包含首頁 |
| 用絕對 URL（含 `https://`） | 不用相對 |
| 一個頁面只有一個 canonical | 多個 = Google 自選或忽略 |
| 重定向後的最終頁要自指 | 不指回原頁 |

### 何時 canonical 指向另一頁

| 場景 | canonical 指向 |
|------|---------------|
| 過濾參數頁（?color=red） | 主商品頁 |
| 列印版 / amp 版 | 主版本 |
| 多 URL 相同內容 | 主 URL |
| 跨網域同內容（如轉貼） | 原始來源 |

## hreflang（多語站必設）

```html
<link rel="alternate" hreflang="zh-TW" href="https://example.com/zh-tw/about">
<link rel="alternate" hreflang="en" href="https://example.com/en/about">
<link rel="alternate" hreflang="ja" href="https://example.com/ja/about">
<link rel="alternate" hreflang="x-default" href="https://example.com/en/about">
```

| 規則 | 說明 |
|------|------|
| **必須雙向** | A 指 B，B 也要指 A |
| 必含 `x-default` | 沒對應語言時的後備 |
| 用 ISO 639-1（語言）+ ISO 3166-1（地區） | `zh-TW` / `zh-CN` 不同 |
| 不要混用語言代碼 | `cht` / `tw` 都不對 |

### Next.js Metadata

```ts
export async function generateMetadata(): Promise<Metadata> {
  return {
    alternates: {
      canonical: '/zh-tw/about',
      languages: {
        'zh-TW': '/zh-tw/about',
        'en': '/en/about',
        'x-default': '/en/about',
      },
    },
  };
}
```

## Pagination 分頁

### 現代做法（2026）

Google 不再使用 `rel=prev/next`，但仍建議保留供其他爬蟲用。**主策略：**

| 方法 | 說明 |
|------|------|
| 每頁 canonical 自指 | 不要把第 2-N 頁 canonical 指第 1 頁（會丟內頁） |
| 篩選參數頁加 noindex | 除非該組合有獨立搜尋意圖 |
| 用 `View All` 頁 | 若可行，提供全列表頁並 canonical 指它 |

### 範例

```html
<!-- /posts?page=2 -->
<link rel="canonical" href="https://example.com/posts?page=2">
<link rel="prev" href="https://example.com/posts?page=1">
<link rel="next" href="https://example.com/posts?page=3">
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|------|------|
| C1 | 每頁 canonical 自指 | 避免被誤判 |
| C2 | hreflang 雙向且含 x-default | 規範要求 |
| C3 | 不在 robots.txt 阻擋 canonical 目標 | 自相矛盾 |
| C4 | canonical 不指向 noindex 頁 | 互斥訊號 |
| C5 | http/https / www 統一一個 | 用 301 redirect 收斂 |

## Anti-Pattern

| Bad | Good |
|---|---|
| 整站 canonical 指首頁 | 每頁自指 |
| 分頁 canonical 都指 page 1 | 每頁自指 |
| http/https 並存 | 301 收斂到 https |
| trailing slash 不一致 | 選一個並 redirect |

## 驗證

- Search Console → 涵蓋範圍 → 看「重複網頁」
- `curl -sI https://example.com/abc | grep -i location`（檢 redirect）
- 看 view-source 內 canonical
