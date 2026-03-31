---
id: backend_frameworks
name: Backend Frameworks
description: Laravel、Node.js、Django、FastAPI 後端框架指南
---

# 🔧 後端框架技能

## 框架選擇指南

| 框架 | 語言 | 適用場景 | 特色 |
|-----|------|---------|-----|
| [Laravel](laravel.md) | PHP | 通用、快速開發 | 全功能、文件完善 |
| [Node.js](node.md) | JavaScript | API、即時應用 | 非同步、輕量 |
| [Django](django.md) | Python | 內容網站、管理系統 | 電池內建 |
| [FastAPI](fastapi.md) | Python | API、微服務 | 高效能、型別安全 |

---

## 快速選擇

### 依專案需求

| 需求 | 推薦 |
|-----|------|
| 快速開發 | Laravel |
| REST API | FastAPI / Express |
| 即時通訊 | Node.js + Socket.io |
| 管理後台 | Django / Laravel |
| 微服務 | FastAPI / Express |
| 全端整合 | Laravel + Inertia |

### 依技術背景

| 背景 | 推薦 |
|-----|------|
| PHP 熟悉 | Laravel |
| Python 熟悉 | Django / FastAPI |
| JavaScript 全端 | Node.js + Express |

---

## API 設計原則

### REST API 規範

| 動作 | HTTP 方法 | 路由 | 說明 |
|-----|----------|------|-----|
| 列表 | GET | /users | 取得所有 |
| 建立 | POST | /users | 建立新資源 |
| 取得 | GET | /users/:id | 取得單個 |
| 更新 | PUT/PATCH | /users/:id | 更新資源 |
| 刪除 | DELETE | /users/:id | 刪除資源 |

### 回應格式

```json
{
  "success": true,
  "data": { ... },
  "message": "操作成功"
}

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "驗證失敗",
    "details": { ... }
  }
}
```

---

## 安全性基本要求

1. **輸入驗證**：永遠不信任用戶輸入
2. **SQL Injection 防護**：使用 ORM 或 Prepared Statements
3. **XSS 防護**：輸出編碼
4. **CSRF 防護**：使用 Token
5. **認證**：JWT 或 Session
6. **HTTPS**：強制使用

---

## 相關技能

- [API 設計](../api/SKILL.md)
- [資料庫](../db/SKILL.md)
- [安全性](../sec/SKILL.md)
