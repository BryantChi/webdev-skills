---
id: database_design
name: database_design
description: Schema 設計、ORM 與 Migration 指南
---

# 🗄️ 資料庫技能

## Schema 設計原則

### 命名規範

| 項目 | 規範 | 範例 |
|-----|------|-----|
| 表名 | 複數、snake_case | users, blog_posts |
| 欄位 | 單數、snake_case | user_id, created_at |
| 主鍵 | id | id |
| 外鍵 | 表名單數_id | user_id, post_id |

### 常用欄位類型

```sql
-- 主鍵
id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY

-- 字串
name VARCHAR(255)
email VARCHAR(255) UNIQUE
content TEXT
status ENUM('draft', 'published')

-- 數字
price DECIMAL(10, 2)
quantity INT UNSIGNED

-- 時間
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
deleted_at TIMESTAMP NULL

-- 布林
is_active BOOLEAN DEFAULT TRUE
```

---

## 關聯類型

### 一對一 (1:1)
```
users.id → profiles.user_id
```

### 一對多 (1:N)
```
users.id → posts.user_id
```

### 多對多 (M:N)
```
posts ← post_tag → tags
```

---

## 索引

```sql
-- 單欄索引
CREATE INDEX idx_email ON users(email);

-- 複合索引
CREATE INDEX idx_status_created ON posts(status, created_at);

-- 唯一索引
CREATE UNIQUE INDEX idx_unique_email ON users(email);
```

### 索引原則

1. 經常 WHERE 的欄位
2. JOIN 的外鍵欄位
3. ORDER BY 的欄位
4. 避免過多索引

---

## ORM 使用

### Eloquent (Laravel)

```php
// 關聯定義
class User extends Model {
    public function posts() {
        return $this->hasMany(Post::class);
    }
}

// 查詢
$users = User::with('posts')
    ->where('status', 'active')
    ->orderBy('created_at', 'desc')
    ->paginate(20);
```

### Prisma (Node.js)

```typescript
const users = await prisma.user.findMany({
  where: { status: 'active' },
  include: { posts: true },
  orderBy: { createdAt: 'desc' },
  take: 20,
});
```

---

## 遷移

### 原則

1. 遷移檔案要有版本控制
2. 每次變更一個遷移
3. 不要修改已執行的遷移
4. 測試環境先執行

### Laravel 範例

```php
// 建立
php artisan make:migration create_posts_table

// 執行
php artisan migrate

// 回滾
php artisan migrate:rollback
```
