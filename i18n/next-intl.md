---
id: web_i18n_next_intl
name: web_i18n_next_intl
description: Next.js App Router + next-intl 完整實作
---

# next-intl（Next.js App Router）

## 安裝

```bash
npm i next-intl
```

## 路由結構

```
app/
├── [locale]/
│   ├── layout.tsx
│   ├── page.tsx
│   └── about/page.tsx
├── i18n.ts
└── middleware.ts

messages/
├── zh-TW.json
└── en.json
```

## middleware.ts

```ts
import createMiddleware from 'next-intl/middleware';

export default createMiddleware({
  locales: ['zh-TW', 'en', 'ja'],
  defaultLocale: 'zh-TW',
  localePrefix: 'always', // 預設語言也加前綴，利於 SEO
});

export const config = {
  matcher: ['/((?!api|_next|.*\\..*).*)'],
};
```

## i18n.ts

```ts
import { getRequestConfig } from 'next-intl/server';
import { notFound } from 'next/navigation';

const locales = ['zh-TW', 'en', 'ja'] as const;

export default getRequestConfig(async ({ locale }) => {
  if (!locales.includes(locale as any)) notFound();
  return { messages: (await import(`./messages/${locale}.json`)).default };
});
```

## app/[locale]/layout.tsx

```tsx
import { NextIntlClientProvider } from 'next-intl';
import { getMessages } from 'next-intl/server';

export default async function LocaleLayout({
  children,
  params: { locale },
}: {
  children: React.ReactNode;
  params: { locale: string };
}) {
  const messages = await getMessages();
  return (
    <html lang={locale}>
      <body>
        <NextIntlClientProvider messages={messages}>{children}</NextIntlClientProvider>
      </body>
    </html>
  );
}
```

## 在元件中使用

```tsx
// Server component
import { useTranslations } from 'next-intl';

export default function Home() {
  const t = useTranslations('home');
  return <h1>{t('title')}</h1>;
}

// Client component（同 API）
'use client';
import { useTranslations } from 'next-intl';
```

## metadata + hreflang

```ts
// app/[locale]/about/page.tsx
import { getTranslations } from 'next-intl/server';

export async function generateMetadata({ params: { locale } }) {
  const t = await getTranslations({ locale, namespace: 'about' });
  return {
    title: t('title'),
    description: t('description'),
    alternates: {
      canonical: `/${locale}/about`,
      languages: {
        'zh-TW': '/zh-TW/about',
        'en': '/en/about',
        'ja': '/ja/about',
        'x-default': '/zh-TW/about',
      },
    },
  };
}
```

## 富文本與插值

```json
// messages/zh-TW.json
{
  "welcome": "你好，{name}",
  "richText": "請<link>閱讀條款</link>",
  "items": "{count, plural, =0 {沒有} one {# 個} other {# 個}}"
}
```

```tsx
t('welcome', { name: 'Bryant' });
t.rich('richText', { link: (chunks) => <Link href="/terms">{chunks}</Link> });
t('items', { count: 3 });
```

## 切換語言元件

```tsx
'use client';
import { useRouter, usePathname } from 'next/navigation';
import { useLocale } from 'next-intl';

export function LangSwitch() {
  const router = useRouter();
  const pathname = usePathname();
  const locale = useLocale();
  const switchTo = (next: string) => {
    const newPath = pathname.replace(`/${locale}`, `/${next}`);
    router.push(newPath);
  };
  return (
    <select value={locale} onChange={(e) => switchTo(e.target.value)}>
      <option value="zh-TW">繁體中文</option>
      <option value="en">English</option>
      <option value="ja">日本語</option>
    </select>
  );
}
```

## CRITICAL 規則

| # | Rule |
|---|---|
| NI1 | `localePrefix: 'always'`，預設語言也加前綴 |
| NI2 | metadata 的 `alternates.languages` 必含所有語言 + x-default |
| NI3 | sitemap 為每個 locale 產生獨立 entry |
| NI4 | `<html lang>` 動態反映 locale |
| NI5 | 翻譯檔放 git，不要動態抓 API（首屏會慢） |
