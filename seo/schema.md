---
id: technical_seo_schema
name: technical_seo_schema
description: JSON-LD 結構化資料（Schema.org）指南
---

# JSON-LD 結構化資料

## 為何用 JSON-LD（不用 Microdata）

Google 官方推薦、與 HTML 解耦、易維護。**全部塞在 `<head>` 內 `<script type="application/ld+json">`**。

## 常用 Schema 類型

| 頁面 | Type | 必填欄位 |
|------|------|---------|
| 首頁 / 全站 | `Organization` / `WebSite` | name, url, logo |
| 部落格文章 | `Article` / `BlogPosting` | headline, datePublished, author, image |
| 商品頁 | `Product` | name, image, offers (price, availability) |
| 食譜 | `Recipe` | name, recipeIngredient, recipeInstructions |
| 影片 | `VideoObject` | name, thumbnailUrl, uploadDate |
| FAQ | `FAQPage` | mainEntity (Question/Answer 陣列) |
| 麵包屑 | `BreadcrumbList` | itemListElement |
| 活動 | `Event` | name, startDate, location |
| 評論星等 | `AggregateRating` | ratingValue, reviewCount |

## Article 範例

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "文章標題",
  "datePublished": "2026-04-26T08:00:00+08:00",
  "dateModified": "2026-04-26T10:00:00+08:00",
  "author": { "@type": "Person", "name": "作者", "url": "https://example.com/about" },
  "publisher": {
    "@type": "Organization",
    "name": "站名",
    "logo": { "@type": "ImageObject", "url": "https://example.com/logo.png" }
  },
  "image": "https://example.com/cover.jpg",
  "mainEntityOfPage": "https://example.com/posts/abc"
}
</script>
```

## Product 範例

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "商品名",
  "image": ["https://example.com/p1.jpg"],
  "description": "...",
  "sku": "ABC-123",
  "brand": { "@type": "Brand", "name": "品牌" },
  "offers": {
    "@type": "Offer",
    "url": "https://example.com/products/abc",
    "priceCurrency": "TWD",
    "price": "1200",
    "availability": "https://schema.org/InStock"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.6",
    "reviewCount": "120"
  }
}
```

## BreadcrumbList

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "首頁", "item": "https://example.com/" },
    { "@type": "ListItem", "position": 2, "name": "分類", "item": "https://example.com/category" },
    { "@type": "ListItem", "position": 3, "name": "商品名", "item": "https://example.com/category/abc" }
  ]
}
```

## Next.js 整合

```tsx
// app/posts/[slug]/page.tsx
export default async function Page({ params }) {
  const post = await getPost(params.slug);
  const jsonLd = { /* ... 如上 ... */ };
  return (
    <>
      <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }} />
      <article>...</article>
    </>
  );
}
```

## 驗證工具

| 工具 | 用途 |
|------|------|
| [Google Rich Results Test](https://search.google.com/test/rich-results) | 看 Google 認得哪些 |
| [Schema.org Validator](https://validator.schema.org/) | 純語法驗 |
| Search Console → 增強功能 | 看實際被收哪些 |

## CRITICAL 規則

| # | Rule |
|---|---|
| SC1 | 一個頁面只塞**該頁實際內容**對應的 schema，不要塞跨頁亂用 |
| SC2 | 日期用 ISO 8601（含時區） |
| SC3 | 圖片用絕對 URL，不要相對路徑 |
| SC4 | `Product.offers.price` 用字串，不要數字 |
| SC5 | 不要 schema spam（瞎標 FAQ、Review 騙星等） |
