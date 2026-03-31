---
id: launch_checklist
name: Launch Checklist
description: 網站上線前的完整檢查清單
---

# ✅ 上線前檢查清單

## 功能測試

- [ ] 所有核心功能正常運作
- [ ] 表單提交正常
- [ ] 連結都可點擊
- [ ] 錯誤頁面 (404, 500) 設定
- [ ] 跨瀏覽器測試 (Chrome, Firefox, Safari, Edge)
- [ ] 行動裝置測試

---

## SEO

- [ ] 每頁有唯一 `<title>`
- [ ] 每頁有 `<meta description>`
- [ ] 正確的標題層級 (h1-h6)
- [ ] 圖片有 alt 屬性
- [ ] sitemap.xml 生成
- [ ] robots.txt 設定
- [ ] Open Graph 標籤
- [ ] 結構化資料 (Schema.org)

---

## 效能

- [ ] Lighthouse 分數 > 90
- [ ] 首次內容繪製 < 1.8s
- [ ] 最大內容繪製 < 2.5s
- [ ] 圖片已壓縮
- [ ] CSS/JS 已壓縮
- [ ] Gzip/Brotli 啟用
- [ ] 快取設定正確

---

## 安全性

- [ ] HTTPS 強制啟用
- [ ] 安全 HTTP Headers 設定
- [ ] 敏感資料不在前端暴露
- [ ] 環境變數不在版控
- [ ] SQL Injection 防護
- [ ] XSS 防護
- [ ] CSRF 防護

---

## 無障礙

- [ ] 色彩對比符合 WCAG AA
- [ ] 可用鍵盤操作
- [ ] 表單有 label
- [ ] 圖片有 alt
- [ ] 焦點樣式可見

---

## 分析與監控

- [ ] Google Analytics 設定
- [ ] 錯誤追蹤設定 (Sentry)
- [ ] 效能監控
- [ ] 伺服器監控

---

## 法律合規

- [ ] 隱私權政策頁面
- [ ] 服務條款頁面
- [ ] Cookie 同意 (GDPR)
- [ ] 版權聲明

---

## 備份與復原

- [ ] 資料庫備份設定
- [ ] 檔案備份設定
- [ ] 回滾計劃準備
- [ ] 災難復原計劃
