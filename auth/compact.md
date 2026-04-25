---
id: web_auth_compact
name: web_auth_compact
description: 認證流派比較 + 必設安全項目
---

# 認證體系精煉版

## 流派比較

| 流派 | 適用 | 優勢 | 風險 |
|------|------|------|------|
| **Cookie Session** | 傳統 SSR | 安全（HttpOnly） | 無分散式時需 sticky session |
| **JWT in Cookie** | SPA + API | 無狀態、可橫向擴展 | 失效困難（除非短時效 + refresh） |
| **JWT in localStorage** | ❌ 不推薦 | — | XSS 直接竊取 |
| **OAuth + Session** | 第三方登入 | 快速整合 | 依賴 provider |
| **Magic Link** | passwordless | 用戶體驗好 | Email 服務依賴 |
| **Passkeys (WebAuthn)** | 高安全 | 無密碼、抗釣魚 | 需新硬體支援 |

## 平台選擇

| 場景 | 推薦 |
|------|------|
| Next App Router 自架 | **Auth.js v5** |
| SaaS 多租戶 + 完整 UI | **Clerk** |
| 一條龍 BaaS | **Supabase Auth** |
| 自架超輕量 | **Lucia** |
| 企業 SSO（SAML/OIDC） | **WorkOS** / **Keycloak** |

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| AU1 | Token / session 存 HttpOnly + Secure + SameSite=Lax cookie | 防 XSS / CSRF |
| AU2 | **不要**把 JWT 存 localStorage | XSS 可竊 |
| AU3 | 密碼用 bcrypt / argon2id（cost ≥ 12） | 抗暴力 |
| AU4 | 登入 / 註冊 endpoint 加 rate limit | 抗刷 |
| AU5 | 重要操作要 re-auth（換 email、刪帳號） | 防 session hijack |
| AU6 | Server action / route handler 必驗權限 | 公開 endpoint |
| AU7 | session timeout（idle + absolute） | 防遺失裝置 |
| AU8 | logout 失效 server side（不只清 cookie） | session blacklist |
| AU9 | OAuth state + PKCE | 防 CSRF + 偽造 |
| AU10 | 不在 client 暴露 secret | 全 server only |

## CSRF 防護

| 方式 | 適用 |
|------|------|
| SameSite=Lax/Strict cookie | 大多數場景（推薦）|
| Double submit token | 跨域需 |
| Origin / Referer 檢查 | 補強 |

## 必設 cookie 屬性

```ts
cookies().set('session', token, {
  httpOnly: true,
  secure: true,            // production
  sameSite: 'lax',         // 或 strict（更嚴）
  path: '/',
  maxAge: 60 * 60 * 24 * 7, // 7 days
});
```

## Anti-Pattern

| Bad | Good |
|---|---|
| JWT 在 localStorage | HttpOnly cookie |
| 密碼用 MD5 / SHA1 | bcrypt / argon2id |
| 登入無 rate limit | 加 |
| logout 只清 client cookie | server 端 invalidate session |
| 用 same secret 永遠不換 | 定期 rotate |
| 機密寫進 NEXT_PUBLIC_ | 用 server-only env |
| 自寫 OAuth 流程 | 用 Auth.js / Arctic |
