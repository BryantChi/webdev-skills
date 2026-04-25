---
id: web_i18n_react_intl
name: web_i18n_react_intl
description: React SPA / Vite 用 react-intl 或 react-i18next
---

# React SPA i18n

## 兩大方案比較

| 套件 | 優勢 | 適合 |
|------|------|------|
| **react-intl**（FormatJS） | ICU 完整、官方標準 | 大型應用、企業 |
| **react-i18next** | 生態廣、外掛多、檔案熱載 | 中小型、快速開發 |

> Next.js App Router 推薦改用 [`next-intl.md`](next-intl.md)。

## react-intl 範例

```bash
npm i react-intl
```

```tsx
// src/i18n.tsx
import { IntlProvider } from 'react-intl';
import { useState, useEffect } from 'react';

const supportedLocales = ['zh-TW', 'en', 'ja'] as const;
type Locale = typeof supportedLocales[number];

export function I18nProvider({ children }) {
  const [locale, setLocale] = useState<Locale>('zh-TW');
  const [messages, setMessages] = useState({});

  useEffect(() => {
    import(`./locales/${locale}.json`).then((m) => setMessages(m.default));
  }, [locale]);

  return (
    <IntlProvider locale={locale} messages={messages} defaultLocale="zh-TW">
      {children}
    </IntlProvider>
  );
}
```

```tsx
import { FormattedMessage, useIntl } from 'react-intl';

function Home() {
  const intl = useIntl();
  return (
    <>
      <h1><FormattedMessage id="home.title" defaultMessage="首頁" /></h1>
      <p>{intl.formatNumber(1234.56, { style: 'currency', currency: 'TWD' })}</p>
      <FormattedMessage
        id="items"
        defaultMessage="{count, plural, =0 {沒有} one {# 個} other {# 個}}"
        values={{ count: 3 }}
      />
    </>
  );
}
```

## react-i18next 範例

```bash
npm i i18next react-i18next i18next-browser-languagedetector i18next-http-backend
```

```ts
// src/i18n.ts
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import LanguageDetector from 'i18next-browser-languagedetector';
import HttpBackend from 'i18next-http-backend';

i18n
  .use(HttpBackend)
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    fallbackLng: 'zh-TW',
    supportedLngs: ['zh-TW', 'en', 'ja'],
    interpolation: { escapeValue: false },
    backend: { loadPath: '/locales/{{lng}}/{{ns}}.json' },
    ns: ['common', 'home', 'auth'],
    defaultNS: 'common',
  });
```

```tsx
import { useTranslation, Trans } from 'react-i18next';

function Home() {
  const { t, i18n } = useTranslation('home');
  return (
    <>
      <h1>{t('title')}</h1>
      <Trans i18nKey="welcome" t={t}>請<a href="/terms">閱讀條款</a></Trans>
      <button onClick={() => i18n.changeLanguage('en')}>EN</button>
    </>
  );
}
```

## CRITICAL 規則

| # | Rule |
|---|---|
| RI1 | 翻譯按 namespace 分檔（home/auth/common） |
| RI2 | SPA 也要在 URL 反映 locale（query 或 hash），利於分享 |
| RI3 | 切換語言後 `<html lang>` 要更新 |
| RI4 | 不在 render path 同步 import 全部 locale | code split |
| RI5 | `formatNumber`/`formatDate` 用內建 ICU，不自寫 |

## 與 Vite 整合（動態 import）

```ts
const messages = import.meta.glob('./locales/*.json');
const loader = (locale: string) => messages[`./locales/${locale}.json`]();
```

## 翻譯流程建議

1. 開發時用 `defaultMessage`（react-intl）或英文 key（i18next）
2. CI 跑 extract（`formatjs extract`）產出 messages 檔
3. 翻譯平台（Crowdin / Lokalise / Locize）回傳
4. CI 驗證 key 完整性（無漏譯）

## SEO 注意

純 SPA + i18n 對 SEO 不友善，**有 SEO 需求請改用 Next.js / Nuxt**（伺服端產 hreflang）。
