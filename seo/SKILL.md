---
id: technical_seo
name: technical_seo
description: 技術 SEO（meta、OG、JSON-LD schema、sitemap、robots、canonical）。不含內容/關鍵字研究
---

# 🔍 技術 SEO

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 必設清單 + 優先級 |
| L2 | [meta-og.md](meta-og.md) | Title / Description / OG / Twitter |
| L2 | [schema.md](schema.md) | JSON-LD 結構化資料 |
| L2 | [sitemap-robots.md](sitemap-robots.md) | sitemap.xml / robots.txt |
| L2 | [canonical.md](canonical.md) | canonical / hreflang / pagination |

## 觸發關鍵字

SEO、search、Google、meta、title、description、OG、schema、JSON-LD、sitemap、robots、canonical、indexable

## Verification

```bash
# 抓 meta
curl -s https://yourdomain | grep -E '<title|og:|twitter:|canonical'
# 驗 schema
curl https://search.google.com/test/rich-results?url=...
# 驗 sitemap
curl https://yourdomain/sitemap.xml | head
```
