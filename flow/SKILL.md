---
id: dev_workflow
name: Dev Workflow
description: Git、Testing、Deploy 與效能優化指南
---

# 🔄 開發流程技能

## Git 工作流程

### 分支策略

```
main (production)
├── develop (開發)
│   ├── feature/user-auth
│   ├── feature/payment
│   └── fix/login-bug
└── hotfix/critical-fix
```

### Commit 訊息規範

```
<type>(<scope>): <subject>

feat: 新功能
fix: 修復 bug
docs: 文件更新
style: 格式調整
refactor: 重構
test: 測試
chore: 雜項
```

範例：
```
feat(auth): 新增 Google 登入功能
fix(cart): 修復數量計算錯誤
docs(readme): 更新安裝說明
```

---

## 測試策略

### 測試金字塔

```
        ╱╲ E2E (少)
       ╱──╲
      ╱ 整合 ╲ (中)
     ╱────────╲
    ╱  單元測試  ╲ (多)
   ╱──────────────╲
```

### 測試類型

| 類型 | 說明 | 工具 |
|-----|------|-----|
| 單元 | 單一函式/元件 | Jest, Vitest |
| 整合 | 模組互動 | Jest, PHPUnit |
| E2E | 完整流程 | Playwright, Cypress |

---

## 部署檢查清單

### 上線前

- [ ] 所有測試通過
- [ ] 程式碼審查完成
- [ ] 效能測試通過
- [ ] 安全檢測完成
- [ ] 文件更新
- [ ] 資料庫遷移準備
- [ ] 環境變數設定
- [ ] 備份計劃

### 上線後

- [ ] 健康檢查
- [ ] 錯誤監控
- [ ] 效能監控
- [ ] 回滾計劃就緒

---

## 效能優化

### 前端

1. **程式碼分割**：動態載入
2. **圖片優化**：WebP、壓縮、lazy load
3. **快取策略**：適當的 Cache-Control
4. **CDN**：靜態資源加速
5. **壓縮**：Gzip/Brotli

### 後端

1. **資料庫索引**
2. **查詢優化**
3. **快取**：Redis
4. **非同步處理**：Queue

---

## 監控

### 指標

- 錯誤率
- 回應時間
- CPU/Memory 使用率
- 請求量

### 工具

- Sentry (錯誤追蹤)
- New Relic (APM)
- Prometheus + Grafana
