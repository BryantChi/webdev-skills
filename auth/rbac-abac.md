---
id: web_auth_rbac_abac
name: web_auth_rbac_abac
description: 權限模型（RBAC 角色 / ABAC 屬性）設計與實作
---

# RBAC / ABAC

## 兩種模型

| 模型 | 邏輯 | 適用 |
|------|------|------|
| **RBAC**（Role-Based） | user 有 role，role 有 permissions | 簡單組織、傳統 |
| **ABAC**（Attribute-Based） | 看 user / resource / context 屬性決定 | 複雜場景、SaaS、多租戶 |
| **ReBAC**（Relationship-Based） | 看實體關聯（如 owner、shared） | 文件系統、SNS |

> 多數應用：以 RBAC 為骨幹，特殊條件用 ABAC 補。

## RBAC 範例（最簡）

```ts
// types
type Role = 'admin' | 'editor' | 'viewer';
type Permission = 'post:create' | 'post:edit' | 'post:delete' | 'user:manage';

// 對應表
const rolePermissions: Record<Role, Permission[]> = {
  admin: ['post:create', 'post:edit', 'post:delete', 'user:manage'],
  editor: ['post:create', 'post:edit'],
  viewer: [],
};

export function hasPermission(role: Role, perm: Permission) {
  return rolePermissions[role].includes(perm);
}
```

```ts
// 使用
'use server';
export async function deletePost(id: string) {
  const session = await auth();
  if (!hasPermission(session.user.role, 'post:delete')) {
    throw new Error('Forbidden');
  }
  await db.posts.delete({ where: { id } });
}
```

## ABAC 範例（policy 函式）

```ts
// 文件規則：可改 = 是 owner 或 admin 或 是 collaborator
async function canEditDoc(user: User, doc: Doc): Promise<boolean> {
  if (user.role === 'admin') return true;
  if (doc.ownerId === user.id) return true;
  const collab = await db.collaborators.findFirst({
    where: { docId: doc.id, userId: user.id, level: { in: ['editor', 'owner'] } },
  });
  return !!collab;
}
```

## 推薦套件

| 套件 | 用途 | 模型 |
|------|------|------|
| [CASL](https://casl.js.org) | JS 通用 | RBAC + ABAC |
| [Oso Cloud](https://www.osohq.com) | SaaS 授權服務 | 全部 |
| [Permit.io](https://www.permit.io) | SaaS | RBAC + ABAC |
| [@permify/permify](https://permify.co) | 開源 | ReBAC（Google Zanzibar 風） |

## CASL 範例（推薦中型專案）

```bash
npm i @casl/ability @casl/react
```

```ts
// abilities.ts
import { defineAbility } from '@casl/ability';

export function defineAbilityFor(user: User) {
  return defineAbility((can, cannot) => {
    if (user.role === 'admin') {
      can('manage', 'all');
      return;
    }
    can('read', 'Post');
    can('create', 'Post');
    can(['update', 'delete'], 'Post', { authorId: user.id });
  });
}
```

```tsx
// 使用
const ability = defineAbilityFor(user);
if (ability.can('delete', post)) {
  // 顯示刪除按鈕
}
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| RB1 | 權限檢查在 server，不只 client | client 只是 UX |
| RB2 | 集中 policy（不要散在各 endpoint） | 易稽核 |
| RB3 | 用 deny-by-default | 預設禁止 |
| RB4 | 權限改變後 invalidate session 或重發 token | 即時生效 |
| RB5 | 留 audit log（誰、何時、做了什麼） | 合規 |
| RB6 | 多租戶：tenant id 必驗（防跨組織存取） | 隔離 |
| RB7 | 不在 JWT 塞所有權限 list（payload 過大） | 用 role + DB lookup |

## 多租戶 ABAC 範例

```ts
async function canViewProject(user: User, project: Project): Promise<boolean> {
  // 1. 必同租戶
  if (user.orgId !== project.orgId) return false;
  // 2. 角色檢查
  if (user.role === 'org:admin') return true;
  // 3. 個別授權
  const member = await db.projectMembers.findUnique({
    where: { userId_projectId: { userId: user.id, projectId: project.id } },
  });
  return !!member;
}
```

## 設計檢查清單

- [ ] 列出所有 resources（Post、User、Org、...）
- [ ] 每個 resource 列出 actions（create/read/update/delete + 自訂）
- [ ] 對應 role × resource × action 矩陣
- [ ] 識別需要 ABAC 的特殊條件（owner / collaborator / time / IP）
- [ ] policy 集中在 `lib/abilities.ts` 或服務
- [ ] 每個 server action / route handler 都有檢查
- [ ] middleware 做粗粒度過濾（角色 / 路徑），細粒度在 handler
- [ ] 留 audit log
- [ ] 寫測試（每個 role × action 至少一個正反例）

## Anti-Pattern

| Bad | Good |
|---|---|
| 只在 client `if (user.role === 'admin')` 顯示按鈕 | server 也驗 |
| 把 permission 全寫在 JWT | role 在 JWT，permission DB 查 |
| 多租戶忘了驗 tenant id | 預設加 where orgId 條件 |
| Hardcode role 字串散落 | 用 enum / const |
| 改 role 後 user 仍能用舊權限 | invalidate session |
