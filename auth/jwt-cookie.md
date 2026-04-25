---
id: web_auth_jwt_cookie
name: web_auth_jwt_cookie
description: JWT 與 Cookie 安全細節（HttpOnly、SameSite、CSRF、refresh token）
---

# JWT / Cookie 安全細節

## Cookie 安全屬性必設

| 屬性 | 值 | 為何 |
|------|-----|------|
| `HttpOnly` | true | 防 JS 讀取（XSS） |
| `Secure` | true | 只走 HTTPS |
| `SameSite` | `Lax`（多數）/ `Strict` | 防 CSRF |
| `Path` | `/` | 適用全站 |
| `Max-Age` / `Expires` | 視場景 | session 期 |
| `Domain` | 不設或設根網域 | 子網域共享需求 |

```ts
// Next.js
import { cookies } from 'next/headers';

cookies().set('session', token, {
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'lax',
  path: '/',
  maxAge: 60 * 60 * 24 * 7,
});
```

## CSRF 防護策略

| 條件 | 解 |
|------|------|
| 同源 | `SameSite=Lax` 已足 |
| 跨子網域 | `SameSite=Strict` + double submit token |
| 跨域 API | CORS allowlist + double submit token + custom header |
| Server actions（Next 15） | 內建 origin check |

### Double Submit Token（跨域時）

```ts
// 設 token cookie（前端可讀）+ 同值放 header
res.cookies.set('csrf', token, { sameSite: 'strict' });
// 客戶端送 X-CSRF-Token header，server 比對
```

## JWT 結構

```
header.payload.signature
```

| 部分 | 內容 |
|------|------|
| header | 演算法（建議 EdDSA / RS256，不用 HS256 + 共享 secret 多服務）|
| payload | `sub`（user id）、`iat`、`exp`、`iss`、`aud` + custom claims |
| signature | 用 secret / private key 簽 |

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| JC1 | JWT 存 HttpOnly cookie，不要 localStorage | XSS 防護 |
| JC2 | access token 短時效（5-15 分鐘） | 失竊損害有限 |
| JC3 | 配 refresh token（rotate + reuse detection） | 安全 + UX |
| JC4 | refresh token 存 HttpOnly + 路徑限定 `/api/auth/refresh` | 隔離 |
| JC5 | 用非對稱簽（RS256/EdDSA），public key 給 verifier | 不洩 secret |
| JC6 | 不在 JWT 塞敏感資料 | payload 是 base64 不是加密 |
| JC7 | 必設 `iss` / `aud` / `exp` | 防誤用 |
| JC8 | refresh 時 detect reuse → 全 session 失效 | 防 token 竊取 |

## Refresh Token Rotation 流程

```
1. 登入：發 access (15min) + refresh (7d) 都進 HttpOnly cookie
2. access 過期：client 自動打 /api/auth/refresh
3. server 驗 refresh：
   - valid + 未用過 → 發新 access + 新 refresh，舊 refresh 標記為已用
   - 已用過 → 全 session 失效（detected reuse = 攻擊跡象）
4. logout：標記 refresh 失效 + 清 cookie
```

## 範例（Next.js）

```ts
// lib/jwt.ts
import { SignJWT, jwtVerify } from 'jose';

const secret = new TextEncoder().encode(process.env.JWT_SECRET!);

export async function signAccess(payload: { sub: string; role: string }) {
  return new SignJWT(payload)
    .setProtectedHeader({ alg: 'HS256' })
    .setIssuedAt()
    .setIssuer('myapp')
    .setExpirationTime('15m')
    .sign(secret);
}

export async function verify(token: string) {
  const { payload } = await jwtVerify(token, secret, { issuer: 'myapp' });
  return payload;
}
```

```ts
// middleware.ts
import { NextResponse } from 'next/server';
import { verify } from './lib/jwt';

export async function middleware(req: Request) {
  const cookie = req.headers.get('cookie') || '';
  const token = cookie.match(/access=([^;]+)/)?.[1];
  if (!token) return NextResponse.redirect(new URL('/login', req.url));
  try {
    await verify(token);
    return NextResponse.next();
  } catch {
    return NextResponse.redirect(new URL('/login', req.url));
  }
}
```

## XSS 影響範圍

即使設 HttpOnly cookie，XSS 仍可：
- 代用戶送請求（cookie 自動帶）
- 改 DOM 釣魚

**故 HttpOnly 是降低衝擊，不是萬靈丹**。同時要：
- CSP（Content-Security-Policy）嚴格
- escape user content
- 重要操作 re-auth

## 常見錯誤

| Bad | Good |
|---|---|
| 把 JWT 存 localStorage | HttpOnly cookie |
| Access token 24 小時 | ≤15 分鐘 + refresh |
| 用 HS256 + 多服務共享 secret | 改非對稱（RS256/EdDSA） |
| JWT 塞 password / 信用卡 | 只放 ID + role |
| Refresh 不 rotate | 必 rotate + reuse detection |
| `SameSite=None` 沒配 `Secure` | 違規，瀏覽器拒收 |
| 同 domain 多 app 共用 session cookie | 路徑或子網域隔離 |
