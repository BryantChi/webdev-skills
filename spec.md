---
id: project_spec_template
name: project_spec_template
description: 完整的專案需求規格書模板，涵蓋從概述到驗收標準
---

# 📋 專案需求規格書

> 複製此模板，填寫您的專案資訊

---

## 1. 專案概述

### 1.1 專案名稱
`[填寫專案名稱]`

### 1.2 專案背景
```
[描述專案的起源、目的，為什麼需要這個專案]
```

### 1.3 專案目標
- [ ] 目標 1：
- [ ] 目標 2：
- [ ] 目標 3：

### 1.4 成功指標
| 指標 | 目標值 | 衡量方式 |
|-----|-------|---------|
| 例：月活躍用戶 | 10,000 | Google Analytics |
| | | |

---

## 2. 範圍定義

### 2.1 包含範圍 (In Scope)
- [ ] 功能 A
- [ ] 功能 B
- [ ] 功能 C

### 2.2 排除範圍 (Out of Scope)
- [ ] 不包含 X
- [ ] 不包含 Y

### 2.3 未來考量 (Future Consideration)
- [ ] Phase 2 功能
- [ ] 待評估功能

---

## 3. 使用者輪廓

### 3.1 主要使用者
| 角色 | 描述 | 需求 |
|-----|------|-----|
| 例：一般會員 | 25-40歲，熟悉網購 | 快速瀏覽、簡單結帳 |
| | | |

### 3.2 使用者旅程
```
[描述使用者從進入網站到完成目標的流程]

1. 使用者進入首頁
2. 瀏覽商品/內容
3. ...
4. 完成目標動作
```

### 3.3 使用者故事
```
作為 [角色]，
我希望能夠 [動作]，
以便 [目的/價值]。
```

| ID | 使用者故事 | 優先級 |
|----|-----------|-------|
| US-001 | 作為訪客，我希望能夠快速註冊，以便開始使用服務 | 高 |
| US-002 | | |

---

## 4. 功能需求

### 4.1 功能清單

#### 🔴 必要功能 (Must Have)
| ID | 功能名稱 | 描述 | 驗收條件 |
|----|---------|------|---------|
| F-001 | 會員註冊 | 支援 Email 註冊 | 可成功建立帳號並收到驗證信 |
| F-002 | | | |

#### 🟡 重要功能 (Should Have)
| ID | 功能名稱 | 描述 | 驗收條件 |
|----|---------|------|---------|
| F-101 | 社群登入 | 支援 Google/FB 登入 | 可使用第三方帳號登入 |
| | | | |

#### 🟢 加分功能 (Nice to Have)
| ID | 功能名稱 | 描述 | 驗收條件 |
|----|---------|------|---------|
| F-201 | 深色模式 | 支援深色主題切換 | 可正常切換且保存偏好 |
| | | | |

### 4.2 功能詳細規格

#### F-001: 會員註冊
```
【輸入】
- Email（必填，格式驗證）
- 密碼（必填，最少 8 字元，需含大小寫及數字）
- 確認密碼（必填，需與密碼相同）
- 同意條款（必選）

【處理流程】
1. 前端驗證格式
2. 檢查 Email 是否已註冊
3. 建立帳號
4. 寄送驗證信
5. 導向驗證提示頁

【輸出】
- 成功：顯示「請至信箱驗證」
- 失敗：顯示對應錯誤訊息

【例外處理】
- Email 已存在：提示「此 Email 已註冊」
- 寄信失敗：提供重寄按鈕
```

---

## 5. 非功能需求

### 5.1 效能需求（對應 [`perf/SKILL.md`](perf/SKILL.md)）

| 項目 | 要求 |
|-----|------|
| LCP（Largest Contentful Paint） | ≤ 2.5s（mobile p75） |
| INP（Interaction to Next Paint） | ≤ 200ms |
| CLS（Cumulative Layout Shift） | ≤ 0.1 |
| TTFB | ≤ 800ms |
| Lighthouse Performance | ≥ 90 |
| 初始 JS bundle | ≤ 150KB（gzip） |
| API 回應時間 | < 500ms (95th percentile) |
| 並發用戶數 | 支援 1,000 同時在線 |
| 圖片載入 | next/image 或 nuxt-image，AVIF/WebP，含 width/height |

### 5.2 安全需求
| 項目 | 要求 |
|-----|------|
| 資料傳輸 | 全站 HTTPS |
| 密碼儲存 | bcrypt 加密，成本因子 ≥ 10 |
| Session | HttpOnly、Secure Cookie |
| API 驗證 | JWT 或 Session Token |
| 輸入驗證 | 防止 XSS、SQL Injection |

### 5.3 可用性需求
| 項目 | 要求 |
|-----|------|
| 系統可用率 | 99.9% (每月停機 < 44 分鐘) |
| 備份頻率 | 每日完整備份 |
| 災難復原 | RTO < 4 小時，RPO < 1 小時 |

### 5.4 相容性需求
| 項目 | 要求 |
|-----|------|
| 瀏覽器 | Chrome、Firefox、Safari、Edge 最新兩版本 |
| 行動裝置 | iOS 14+、Android 10+ |
| 響應式 | 支援 320px - 2560px 螢幕寬度 |

### 5.5 無障礙需求（對應 [`test/a11y-axe.md`](test/a11y-axe.md)、[`check/a11y.md`](check/a11y.md)）

| 項目 | 要求 |
|-----|------|
| WCAG 等級 | 2.2 AA |
| 鍵盤操作 | 所有功能可用鍵盤完成、`:focus-visible` 顯著輪廓 |
| 螢幕閱讀器 | 支援 NVDA、VoiceOver |
| 色彩對比 | 文字 ≥ 4.5:1，UI 元件 ≥ 3:1 |
| 自動化驗證 | axe-core 規則 (wcag22aa) 零違規 |
| 觸控目標 | ≥ 44×44px（WCAG 2.2 新增） |
| reduced motion | 支援 `prefers-reduced-motion` |

### 5.6 測試需求（對應 [`test/SKILL.md`](test/SKILL.md)）

| 項目 | 要求 |
|-----|------|
| 單元測試覆蓋率 | ≥ 80%（lines / branches） |
| E2E 涵蓋 | 主要使用者旅程 100%（登入、結帳、表單送出） |
| 工具 | Vitest（單元）+ Playwright（E2E）+ axe-core（a11y） |
| CI 整合 | PR 必跑 lint / typecheck / test / build / e2e |
| 視覺回歸 | 關鍵頁面 baseline 鎖定（可選） |

### 5.7 SEO 需求（對應 [`seo/SKILL.md`](seo/SKILL.md)）

| 項目 | 要求 |
|-----|------|
| 每頁 metadata | title 50-60 字、description 70-160 字 |
| canonical | 每頁自指 canonical |
| 結構化資料 | JSON-LD（Article / Product / Organization 視內容） |
| sitemap | 自動產出 `sitemap.xml` |
| robots.txt | 設妥 + 含 sitemap URL |
| OG image | 1200×630 PNG/JPG |
| 多語 hreflang | 雙向 + x-default（多語站） |
| Lighthouse SEO | ≥ 95 |

### 5.8 可觀測性需求（對應 [`obs/SKILL.md`](obs/SKILL.md)）

| 項目 | 要求 |
|-----|------|
| 錯誤追蹤 | Sentry（含 release tag、source map） |
| RUM | web-vitals 上報（Vercel Analytics / 自架）|
| 結構化 logging | pino / winston，redact PII |
| Alert | 5xx > 1%、LCP > 4s（field p75）、新錯誤類型出現 |
| Uptime | Better Stack / UptimeRobot 健康檢查 |

---

## 6. 技術規格

### 6.1 技術棧
| 層級 | 技術選擇 | 版本 |
|-----|---------|------|
| 前端框架 | React / Vue / Next.js / Nuxt | |
| 元件庫 | shadcn/ui + Radix（推薦）/ MUI / 自建 | |
| CSS 方案 | Tailwind / Bootstrap / SCSS | |
| 後端框架 | Laravel / Node.js / Django / FastAPI | |
| 資料庫 | MySQL / PostgreSQL / MongoDB | |
| 快取 | Redis | |
| 檔案儲存 | S3 / 本機 | |
| 多語系 | next-intl / vue-i18n / @nuxtjs/i18n | |
| 部署 | Vercel / Netlify / Docker / 自架 | |
| CI/CD | GitHub Actions / GitLab CI | |
| 錯誤追蹤 | Sentry | |
| RUM | Vercel Analytics / web-vitals + 自建 | |

### 6.2 第三方服務
| 服務 | 用途 | 備註 |
|-----|------|-----|
| 例：Stripe | 金流 | 國際信用卡 |
| 例：綠界 | 金流 | 台灣在地 |
| 例：Firebase | 推播 | |

### 6.3 環境需求
| 環境 | 規格 | 用途 |
|-----|------|-----|
| 開發 | 本機 | 開發測試 |
| 測試 | 2 vCPU, 4GB RAM | QA 測試 |
| 正式 | 4 vCPU, 8GB RAM | 生產環境 |

---

## 7. UI/UX 需求

### 7.1 設計風格
- 主風格：`[從 55 種風格中選擇]`
- 備選風格：

### 7.2 色彩規範
| 用途 | 色碼 | 說明 |
|-----|------|-----|
| 主色 | #______ | |
| 輔色 | #______ | |
| 強調色 | #______ | |
| 背景色 | #______ | |
| 文字色 | #______ | |

### 7.3 字型規範
| 用途 | 字型 | 大小 | 字重 |
|-----|-----|------|-----|
| 標題 | | | |
| 內文 | | | |
| 按鈕 | | | |

### 7.4 頁面清單
| 頁面 | 路由 | 說明 | 優先級 |
|-----|------|-----|-------|
| 首頁 | / | | 高 |
| 登入 | /login | | 高 |
| | | | |

---

## 8. 時程規劃

### 8.1 里程碑
| 階段 | 日期 | 交付項目 |
|-----|------|---------|
| 規劃完成 | | 需求規格書定稿 |
| 設計完成 | | UI/UX 設計稿 |
| 開發完成 | | 功能開發完畢 |
| 測試完成 | | 所有 Bug 修復 |
| 正式上線 | | 生產環境部署 |

### 8.2 甘特圖 (概略)
```
Week    1    2    3    4    5    6    7    8
規劃    ████
設計         ████
開發              ████████████
測試                        ████████
部署                                  ████
```

---

## 9. 驗收標準

### 9.1 功能驗收
- [ ] 所有必要功能 (Must Have) 完成
- [ ] 所有使用者故事可正常完成
- [ ] 無 P0/P1 等級 Bug

### 9.2 效能驗收
- [ ] Lighthouse Performance ≥ 90、SEO ≥ 95、Accessibility ≥ 95
- [ ] Core Web Vitals 全部通過（field p75: LCP ≤2.5s, INP ≤200ms, CLS ≤0.1）
- [ ] 跑完 `check/lighthouse.md` 清單
- [ ] 壓力測試通過

### 9.3 安全驗收
- [ ] OWASP Top 10 檢測通過
- [ ] 滲透測試通過
- [ ] SSL 憑證正確安裝
- [ ] 機密未進 git（用 secret manager）

### 9.4 相容性驗收
- [ ] 所有目標瀏覽器測試通過
- [ ] 所有目標裝置測試通過
- [ ] 響應式設計正常

### 9.5 測試驗收（對應 [`test/SKILL.md`](test/SKILL.md)）
- [ ] 單元測試覆蓋 ≥ 80%
- [ ] 主流程 E2E 全綠
- [ ] axe-core 零違規
- [ ] CI 全通過

### 9.6 SEO 驗收（對應 [`check/seo.md`](check/seo.md)）
- [ ] 每頁 metadata 唯一
- [ ] sitemap.xml 已提交 Search Console
- [ ] [Rich Results Test](https://search.google.com/test/rich-results) 通過
- [ ] canonical / hreflang 設妥

### 9.7 可觀測性驗收（對應 [`obs/SKILL.md`](obs/SKILL.md)）
- [ ] Sentry 接妥含 release tag
- [ ] Web Vitals RUM 上報運作中
- [ ] Alert 規則設妥（5xx、效能、新錯誤）
- [ ] Uptime 監控運作中

---

## 10. 附錄

### 10.1 術語對照
| 術語 | 說明 |
|-----|------|
| | |

### 10.2 參考資料
- 連結 1
- 連結 2

### 10.3 修訂紀錄
| 版本 | 日期 | 修改者 | 修改內容 |
|-----|------|-------|---------|
| 1.0 | | | 初版建立 |
