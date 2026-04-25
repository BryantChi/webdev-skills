---
id: web_security
name: web_security
description: 驗證、授權與 OWASP 安全檢查清單
---

# 🔒 安全性技能

## 常見攻擊防護

### SQL Injection

❌ **危險**
```php
$query = "SELECT * FROM users WHERE id = " . $_GET['id'];
```

✅ **安全**
```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$id]);
```

### XSS (跨站腳本)

❌ **危險**
```html
<div><?= $userInput ?></div>
```

✅ **安全**
```html
<div><?= htmlspecialchars($userInput, ENT_QUOTES, 'UTF-8') ?></div>
```

### CSRF (跨站請求偽造)

```html
<!-- 表單中加入 CSRF Token -->
<form method="POST">
  <input type="hidden" name="_token" value="<?= csrf_token() ?>">
</form>
```

---

## 認證安全

### 密碼處理

```php
// 雜湊密碼
$hash = password_hash($password, PASSWORD_BCRYPT, ['cost' => 12]);

// 驗證密碼
if (password_verify($password, $hash)) {
    // 正確
}
```

### Session 安全

```php
session_regenerate_id(true); // 登入後重新產生 ID
session_set_cookie_params([
    'lifetime' => 0,
    'path' => '/',
    'secure' => true,     // HTTPS only
    'httponly' => true,   // 禁止 JS 存取
    'samesite' => 'Lax'
]);
```

### JWT 注意事項

1. 使用強密鑰
2. 設定合理過期時間
3. 考慮 Token 撤銷機制
4. 儲存在 httpOnly Cookie

---

## 輸入驗證

```php
// 伺服器端驗證（必須）
$rules = [
    'email' => 'required|email|max:255',
    'password' => 'required|min:8',
];

// 永遠不要信任前端驗證
```

---

## HTTP Headers

```
# 安全標頭
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Security-Policy: default-src 'self'
```

---

## 安全檢查清單

- [ ] HTTPS 全站強制
- [ ] 密碼使用 bcrypt/argon2 雜湊
- [ ] 所有輸入都有驗證
- [ ] 使用參數化查詢
- [ ] 輸出都有跳脫
- [ ] CSRF Token 保護
- [ ] 安全 HTTP Headers
- [ ] 敏感資料加密
- [ ] 錯誤訊息不洩漏細節
- [ ] 定期更新依賴套件

---

## 相關技能

- [認證授權](../auth/SKILL.md)（深入：Auth.js、Clerk、JWT-Cookie 細節、RBAC/ABAC）
- [JWT / Cookie 安全細節](../auth/jwt-cookie.md)（HttpOnly、SameSite、refresh rotation）
- [權限模型](../auth/rbac-abac.md)
- [API 設計](../api/SKILL.md)
- [Next 安全規則](../fe/next-rules.critical.md) NX22-25（route handler 驗 input、CSP）
- [部署機密管理](../deploy/compact.md)（secret manager）
- [錯誤監控（不洩漏 PII）](../obs/logging.md)
- [通用反模式](../_shared/anti-patterns.md)（XSS / CSRF / SQL Injection 段）

> **核心原則**：deny-by-default、最小權限、伺服端必驗（不信任 client）、所有秘鑰用 secret manager。
