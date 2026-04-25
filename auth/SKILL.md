---
id: web_auth
name: web_auth
description: 認證授權（Auth.js、Clerk、JWT/Cookie 安全、RBAC/ABAC）。不含 OAuth provider 細節
---

# 🔐 認證授權

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)
> 安全細節亦見 [`../sec/SKILL.md`](../sec/SKILL.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 流派比較 + 必設 |
| L2 | [auth-js.md](auth-js.md) | Auth.js v5（Next 主流） |
| L2 | [clerk.md](clerk.md) | Clerk（SaaS 多租戶） |
| L2 | [jwt-cookie.md](jwt-cookie.md) | JWT / Cookie 安全細節 |
| L2 | [rbac-abac.md](rbac-abac.md) | 角色/屬性權限模型 |

## 觸發關鍵字

auth、login、登入、註冊、session、JWT、cookie、OAuth、SSO、RBAC、ABAC、Clerk、Auth.js、NextAuth

## Verification

- HttpOnly + Secure + SameSite cookie
- Server action / route handler 內驗權限
- Rate limit on login / signup endpoints
