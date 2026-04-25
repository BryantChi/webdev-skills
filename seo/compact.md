---
id: technical_seo_compact
name: technical_seo_compact
description: 技術 SEO 必設清單與優先級
---

# 技術 SEO 必設清單

## CRITICAL（沒這些 = 沒 SEO）

| # | 項目 | 範例 |
|---|------|------|
| S1 | `<title>` 30-60 字 | 動態組裝 `頁面 - 站名` |
| S2 | `<meta name="description">` 70-160 字 | 寫人看的不是塞關鍵字 |
| S3 | `<link rel="canonical">` 每頁 | 避免重複內容懲罰 |
| S4 | `og:title / og:description / og:image` | 社群分享必備 |
| S5 | `<meta name="viewport">` | mobile-friendly 信號 |
| S6 | `<html lang="zh-Hant-TW">` | 多語必設 |
| S7 | `sitemap.xml` + `robots.txt` | GSC 提交 |
| S8 | 結構化資料（JSON-LD） | Article / Product / Organization |
| S9 | h1 唯一且包含主要意圖 | 不是 logo 圖那種 |
| S10 | 內部連結用 `<a href>` 真連結 | 不用 onClick 假連結 |

## HIGH

| # | 項目 |
|---|------|
| S11 | `og:image` 1200×630 PNG/JPG 帶品牌 |
| S12 | Twitter card `summary_large_image` |
| S13 | 多語：`hreflang` 每語版本 |
| S14 | 分頁：`rel=prev/next`（仍建議）+ canonical 自指 |
| S15 | RSS / Atom feed（部落格） |
| S16 | 404 真的回 404，不要 200 + 空頁 |
| S17 | 移除 dev mock 路由（noindex） |

## MEDIUM

| # | 項目 |
|---|------|
| S18 | 圖片 alt 含描述（不堆關鍵字） |
| S19 | URL 短、語意化、kebab-case |
| S20 | breadcrumb + JSON-LD BreadcrumbList |
| S21 | `<link rel="preconnect">` 給 CDN/字型 |

## Anti-Pattern

| Bad | Good |
|---|---|
| 客戶端動態 inject `<title>`（Next CSR） | SSR / RSC 內 metadata |
| 整站 canonical 指首頁 | 每頁 canonical 自指 |
| 重複 description 全站一樣 | 每頁獨特 |
| `<meta keywords>` | 早已不被 Google 採用 |
| robots.txt block 全部 | 只 block 不該爬的（admin、search） |
