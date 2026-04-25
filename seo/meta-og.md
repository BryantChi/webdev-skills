---
id: technical_seo_meta_og
name: technical_seo_meta_og
description: Meta、OG、Twitter card 完整指南
---

# Meta / OG / Twitter

## 完整範例

```html
<!-- 基本 -->
<title>商品名稱 - 分類 - 站名</title>
<meta name="description" content="70-160 字的人讀句子，描述本頁價值">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="canonical" href="https://example.com/products/abc">

<!-- Open Graph -->
<meta property="og:type" content="product"> <!-- website / article / product -->
<meta property="og:title" content="商品名稱">
<meta property="og:description" content="社群預覽用描述">
<meta property="og:image" content="https://example.com/og/abc.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:url" content="https://example.com/products/abc">
<meta property="og:locale" content="zh_TW">

<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@brand">
<meta name="twitter:title" content="商品名稱">
<meta name="twitter:description" content="...">
<meta name="twitter:image" content="https://example.com/og/abc.png">
```

## Next.js App Router

```ts
// app/products/[slug]/page.tsx
import type { Metadata } from 'next';

export async function generateMetadata({ params }): Promise<Metadata> {
  const product = await getProduct(params.slug);
  return {
    title: `${product.name} - 站名`,
    description: product.summary,
    alternates: { canonical: `/products/${params.slug}` },
    openGraph: {
      title: product.name,
      description: product.summary,
      images: [{ url: product.ogImage, width: 1200, height: 630 }],
      locale: 'zh_TW',
      type: 'website',
    },
    twitter: { card: 'summary_large_image', images: [product.ogImage] },
  };
}
```

## Nuxt 3

```ts
useSeoMeta({
  title: `${product.name} - 站名`,
  description: product.summary,
  ogTitle: product.name,
  ogImage: product.ogImage,
  twitterCard: 'summary_large_image',
});
```

## OG Image 動態生成

| 工具 | 備註 |
|------|------|
| `next/og` | Next 內建 ImageResponse |
| `@vercel/og` | edge runtime |
| `satori` | 通用 |

範例（Next）：

```tsx
// app/og/route.tsx
import { ImageResponse } from 'next/og';
export async function GET() {
  return new ImageResponse(
    <div style={{ background: '#000', color: '#fff', fontSize: 64 }}>Hello</div>,
    { width: 1200, height: 630 }
  );
}
```

## 字數標準

| 欄位 | 最佳長度 |
|------|---------|
| title | 50-60 字元（中文 25-30） |
| description | 120-160 字元（中文 60-80） |
| og:image | 1200×630（1.91:1） |

## 驗證

- [ ] [opengraph.xyz](https://opengraph.xyz) 預覽
- [ ] [Twitter Card Validator](https://cards-dev.twitter.com/validator)
- [ ] [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)
