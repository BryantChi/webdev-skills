---
name: SEO 檢查清單
description: 搜尋引擎優化完整檢查清單
---

# 🔍 SEO 檢查清單

## 基礎設定

- [ ] 每頁有唯一 `<title>` (50-60 字元)
- [ ] 每頁有 `<meta description>` (150-160 字元)
- [ ] 使用 `<meta charset="UTF-8">`
- [ ] 設定 `<meta viewport>`
- [ ] 設定 canonical URL

```html
<title>頁面標題 | 網站名稱</title>
<meta name="description" content="頁面描述...">
<link rel="canonical" href="https://example.com/page">
```

---

## 結構化內容

- [ ] 每頁只有一個 `<h1>`
- [ ] 標題層級正確 (h1 > h2 > h3)
- [ ] 使用語意化 HTML
- [ ] 結構化資料 (Schema.org)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "公司名稱",
  "url": "https://example.com"
}
</script>
```

---

## 技術 SEO

- [ ] sitemap.xml 生成並提交
- [ ] robots.txt 正確設定
- [ ] 使用 HTTPS
- [ ] 行動裝置友善
- [ ] 頁面載入速度 < 3 秒
- [ ] Core Web Vitals 通過

```
# robots.txt
User-agent: *
Allow: /
Sitemap: https://example.com/sitemap.xml
```

---

## 社群分享

### Open Graph

```html
<meta property="og:title" content="標題">
<meta property="og:description" content="描述">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="website">
```

### Twitter Cards

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="標題">
<meta name="twitter:description" content="描述">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

---

## 圖片優化

- [ ] 使用描述性檔名
- [ ] 所有圖片有 alt 屬性
- [ ] 圖片已壓縮
- [ ] 使用現代格式 (WebP)
- [ ] 設定 width/height 避免 CLS

---

## 連結

- [ ] 內部連結結構良好
- [ ] 無斷裂連結 (404)
- [ ] 外部連結使用 rel="noopener"
- [ ] 重要頁面可在 3 次點擊內到達

---

## 國際化

- [ ] 設定 `<html lang="zh-TW">`
- [ ] 多語言使用 hreflang
```html
<link rel="alternate" hreflang="zh-TW" href="https://example.com/zh/">
<link rel="alternate" hreflang="en" href="https://example.com/en/">
```

---

## 工具

1. **Google Search Console** - 提交 sitemap
2. **Google PageSpeed Insights** - 效能分析
3. **Screaming Frog** - SEO 爬蟲
4. **Ahrefs/SEMrush** - 競爭分析
