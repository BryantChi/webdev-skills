---
name: 頁面模板
description: 常用頁面佈局的程式碼模板
---

# 📄 頁面模板

## Landing Page 結構

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>產品名稱 | 標語</title>
  <meta name="description" content="產品描述...">
</head>
<body>
  <!-- 導航 -->
  <nav class="navbar">...</nav>

  <!-- Hero 區塊 -->
  <section class="hero">
    <h1>主標題</h1>
    <p>副標題描述</p>
    <a href="#" class="btn btn-primary">立即開始</a>
  </section>

  <!-- 特色 -->
  <section class="features">
    <h2>特色功能</h2>
    <div class="features-grid">...</div>
  </section>

  <!-- 方案 -->
  <section class="pricing">
    <h2>pricing方案</h2>
    <div class="pricing-grid">...</div>
  </section>

  <!-- CTA -->
  <section class="cta">
    <h2>準備好了嗎？</h2>
    <a href="#" class="btn btn-primary">開始使用</a>
  </section>

  <!-- 頁尾 -->
  <footer class="footer">...</footer>
</body>
</html>
```

---

## Dashboard 佈局

```html
<div class="dashboard">
  <!-- 側邊欄 -->
  <aside class="sidebar">
    <div class="sidebar-header">Logo</div>
    <nav class="sidebar-nav">
      <a href="#" class="sidebar-link active">首頁</a>
      <a href="#" class="sidebar-link">訂單</a>
      <a href="#" class="sidebar-link">設定</a>
    </nav>
  </aside>

  <!-- 主內容 -->
  <main class="main-content">
    <header class="topbar">
      <h1 class="page-title">Dashboard</h1>
      <div class="user-menu">...</div>
    </header>

    <div class="content">
      <!-- 統計卡片 -->
      <div class="stats-grid">
        <div class="stat-card">...</div>
      </div>

      <!-- 圖表 -->
      <div class="chart-container">...</div>

      <!-- 表格 -->
      <div class="table-container">...</div>
    </div>
  </main>
</div>
```

```css
.dashboard {
  display: grid;
  grid-template-columns: 250px 1fr;
  min-height: 100vh;
}

.sidebar {
  background: var(--color-gray-900);
  color: white;
  padding: 1.5rem;
}

.main-content {
  background: var(--color-gray-100);
}

.topbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background: white;
  border-bottom: 1px solid var(--color-gray-200);
}

.content {
  padding: 2rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.5rem;
  margin-bottom: 2rem;
}
```

---

## 認證頁面

```html
<div class="auth-page">
  <div class="auth-card">
    <div class="auth-header">
      <h1>登入</h1>
      <p>歡迎回來！請登入您的帳號。</p>
    </div>

    <form class="auth-form">
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" class="input">
      </div>
      <div class="form-group">
        <label for="password">密碼</label>
        <input type="password" id="password" class="input">
      </div>
      <button type="submit" class="btn btn-primary btn-block">登入</button>
    </form>

    <div class="auth-footer">
      <p>還沒有帳號？<a href="/register">註冊</a></p>
    </div>
  </div>
</div>
```

```css
.auth-page {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--color-gray-100);
  padding: 2rem;
}

.auth-card {
  background: white;
  padding: 2.5rem;
  border-radius: 1rem;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  width: 100%;
  max-width: 400px;
}

.auth-header {
  text-align: center;
  margin-bottom: 2rem;
}

.btn-block {
  width: 100%;
}

.auth-footer {
  text-align: center;
  margin-top: 1.5rem;
  color: var(--color-gray-600);
}
```

---

## 文章頁面

```html
<article class="article">
  <header class="article-header">
    <h1 class="article-title">文章標題</h1>
    <div class="article-meta">
      <span class="author">作者名稱</span>
      <span class="date">2024-01-01</span>
    </div>
  </header>

  <div class="article-content prose">
    <p>文章內容...</p>
    <h2>小標題</h2>
    <p>更多內容...</p>
  </div>

  <footer class="article-footer">
    <div class="tags">
      <a href="#" class="tag">標籤</a>
    </div>
  </footer>
</article>
```

```css
.article {
  max-width: 720px;
  margin: 0 auto;
  padding: 2rem;
}

.article-title {
  font-size: 2.5rem;
  line-height: 1.2;
  margin-bottom: 1rem;
}

.article-meta {
  color: var(--color-gray-500);
  margin-bottom: 2rem;
}

.prose {
  font-size: 1.125rem;
  line-height: 1.8;
  color: var(--color-gray-700);
}

.prose h2 {
  font-size: 1.5rem;
  margin-top: 2rem;
  margin-bottom: 1rem;
}

.prose p {
  margin-bottom: 1.5rem;
}
```
