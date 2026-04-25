---
id: web_i18n_compact
name: web_i18n_compact
description: i18n 路由策略 + 訊息結構 + 必裝清單
---

# i18n 精煉版

## 路由策略選擇

| 策略 | 範例 | SEO | 適用 |
|------|------|-----|------|
| 子目錄 | `/zh-tw/about` | ⭐⭐⭐ 推薦 | 大多數 |
| 子網域 | `tw.example.com` | ⭐⭐ 較複雜 | 多區域品牌 |
| 參數 | `?lang=tw` | ⭐ 不推薦 | — |
| 自動切換不換 URL | (cookie) | ❌ 反對 | 內部後台才行 |

**首選：子目錄 + 預設語言不省略前綴**（避免重複內容問題）。

## 必裝清單

| 框架 | 套件 |
|---|---|
| Next | `next-intl` |
| Vue 3 | `vue-i18n@9` |
| Nuxt 3 | `@nuxtjs/i18n` |
| React SPA | `react-intl` 或 `i18next` + `react-i18next` |
| 通用 | `@formatjs/intl` 處理日期/數字/複數 |

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| I1 | 翻譯字串用 ICU MessageFormat | 自寫 `if singular...` | `{count, plural, one {# item} other {# items}}` |
| I2 | 日期/數字用 `Intl.DateTimeFormat` / `NumberFormat` | 自寫 format | 內建 |
| I3 | 不在程式碼硬塞語系字串 | `if (lang === 'tw') ...` | 抽出 messages 檔 |
| I4 | hreflang + canonical 配對 | 漏一個 | 雙向且含 x-default |
| I5 | 翻譯檔分塊（按頁面/功能） | 一個大 JSON | `home.json` `auth.json` |
| I6 | RTL 支援 `dir="rtl"`（阿拉伯文等） | 不處理 | `<html dir>` 動態 |
| I7 | locale 在 URL 而非 cookie | 不利分享 | `/en/about` |

## 訊息檔結構

```
locales/
├── zh-TW/
│   ├── common.json
│   ├── home.json
│   └── auth.json
├── en/
│   └── ...
└── ja/
    └── ...
```

## ICU 範例

```json
{
  "items": "{count, plural, =0 {沒有項目} one {# 個項目} other {# 個項目}}",
  "lastSeen": "上次登入：{date, date, long}",
  "price": "{amount, number, ::currency/TWD}"
}
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 翻譯內含 HTML 字串 | 用富文本 component（next-intl `<rich>` / vue-i18n `<i18n-t>`） |
| 串接句子片段 | 整句一個 key（語序不同） |
| 用 key 作為文字（fallback） | 缺翻譯時 fallback 到預設語言 |
| 把 timezone 寫死台北 | 用使用者 timezone |
