---
id: technical_seo_sitemap_robots
name: technical_seo_sitemap_robots
description: sitemap.xml 與 robots.txt 完整指南
---

# Sitemap / Robots

## sitemap.xml 範例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2026-04-26</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/posts/abc</loc>
    <lastmod>2026-04-20</lastmod>
  </url>
</urlset>
```

> `priority` 與 `changefreq` Google 已忽略，但保留無妨。`lastmod` 重要。

## 多語 sitemap（hreflang）

```xml
<url>
  <loc>https://example.com/zh-tw/about</loc>
  <xhtml:link rel="alternate" hreflang="zh-TW" href="https://example.com/zh-tw/about"/>
  <xhtml:link rel="alternate" hreflang="en" href="https://example.com/en/about"/>
  <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/en/about"/>
</url>
```

需在 `<urlset>` 加 `xmlns:xhtml="http://www.w3.org/1999/xhtml"`。

## Sitemap Index（>50K URLs 或 >50MB）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://example.com/sitemap-posts.xml</loc>
    <lastmod>2026-04-26</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://example.com/sitemap-products.xml</loc>
  </sitemap>
</sitemapindex>
```

## Next.js App Router 自動產生

```ts
// app/sitemap.ts
import type { MetadataRoute } from 'next';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const posts = await getAllPosts();
  return [
    { url: 'https://example.com/', lastModified: new Date(), priority: 1 },
    ...posts.map((p) => ({
      url: `https://example.com/posts/${p.slug}`,
      lastModified: p.updatedAt,
    })),
  ];
}
```

## robots.txt 範例

```
User-agent: *
Allow: /
Disallow: /admin
Disallow: /api/
Disallow: /search?
Disallow: /*.json$

Sitemap: https://example.com/sitemap.xml
```

### Next.js App Router

```ts
// app/robots.ts
import type { MetadataRoute } from 'next';

export default function robots(): MetadataRoute.Robots {
  return {
    rules: [{ userAgent: '*', allow: '/', disallow: ['/admin', '/api/'] }],
    sitemap: 'https://example.com/sitemap.xml',
  };
}
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|------|------|
| R1 | sitemap.xml URL 全絕對路徑 | 規範要求 |
| R2 | 不收 noindex / 重定向 / 404 頁 | 浪費 crawl budget |
| R3 | `lastmod` 真實反映更新 | 假資料反而扣分 |
| R4 | robots.txt 不 block CSS/JS | Google 需渲染 |
| R5 | Search Console 提交 sitemap | 主動告知 |
| R6 | 大站分檔（新聞、商品、靜態頁分開） | 易監控 |

## Anti-Pattern

| Bad | Good |
|---|---|
| `Disallow: /` 全擋 | 只擋 admin / private |
| sitemap 含 query string 大量重複 | 用 canonical 收斂 |
| 把 dev / staging 也放進去 | 各環境獨立 |
| sitemap 動態實時生成（每 request） | cache 或 build time 產 |

## 多 sitemap 切分建議

| 內容類型 | 檔案 |
|---------|------|
| 文章 | sitemap-posts.xml |
| 產品 | sitemap-products.xml |
| 分類 | sitemap-categories.xml |
| 圖片 | sitemap-images.xml |
| 影片 | sitemap-videos.xml |
| News（新聞站） | sitemap-news.xml（48 小時內） |
