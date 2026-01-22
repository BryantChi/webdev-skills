---
description: 使用網站開發 Skills 系統建立完整網站的標準工作流程
---

# 🚀 網站開發完整工作流程

> 當你說「幫我建立網站」或「我想做一個網站」時，按照此流程執行

---

## 🎯 快速啟動

**對 AI 說以下任一句話即可啟動完整流程：**

```text
幫我建立一個網站
我想做一個 [描述] 網站
使用 webdev skill 建立網站
啟動網站開發流程
```

---

## 📋 完整工作流程

### Phase 1：需求收集 (5-10 分鐘)

// turbo-all

**AI 需詢問用戶：**

```text
1. 這個網站的目的是什麼？
   □ 企業形象  □ 電商  □ 部落格  □ SaaS  □ 作品集  □ 其他

2. 主要目標受眾是誰？
   □ 年齡層  □ 職業  □ 興趣

3. 有沒有喜歡的參考網站？（最多 3 個）

4. 有沒有必須使用的品牌色或 Logo？

5. 有沒有必須使用或避免的技術？

6. 預計的頁面有哪些？

7. 有沒有特殊功能需求？
   □ 會員系統  □ 金流  □ 表單  □ 多語言  □ 其他
```

→ 根據回答，讀取 `webdev/spec.md` 填寫需求規格書

---

### Phase 2：技術棧選擇 (3-5 分鐘)

**讀取並執行：** `webdev/wizard/tech.md`

根據 Phase 1 的答案，執行技術棧精靈的 12 個問題（或根據明確需求直接推薦）

**輸出：**

- 前端框架選擇
- CSS 方案選擇
- 後端框架選擇（如需要）
- 資料庫選擇（如需要）

---

### Phase 3：設計風格確定 (3-5 分鐘)

**讀取並執行：** `webdev/wizard/style.md`

根據 Phase 1 的答案，執行風格精靈的 15 個問題（或根據明確偏好直接推薦）

**輸出：**

- 主要設計風格
- 配色方案
- 字型建議
- 動態效果建議

---

### Phase 4：Coding Style 確定 (2-3 分鐘)

**讀取並執行：** `webdev/wizard/code.md`

**輸出：**

- ESLint/Prettier 設定
- 命名規範
- 檔案結構規範

---

### Phase 5：專案初始化

**根據選擇的技術棧，執行對應的初始化：**

```bash
# Next.js 範例
npx create-next-app@latest ./my-project --typescript --tailwind --app

# Vue 範例
npm create vue@latest

# Laravel 範例
composer create-project laravel/laravel my-project
```

**設定檔案：**

- 讀取對應的 `webdev/code/*.md` 設定 linting
- 建立 `.editorconfig`
- 建立 `README.md`

---

### Phase 6：設計系統建立

**必須讀取：**

1. `webdev/ui/no-ai-feel.md` ← **重要：確保設計無 AI 感**
2. `webdev/ui/color.md`
3. `webdev/ui/typo.md`
4. `webdev/ui/space.md`
5. 選定的風格文件 `webdev/ui/styles/XX-name.md`

**建立：**

```text
styles/
├── globals.css          # 全域樣式 + CSS 變數
├── design-tokens.css    # 設計 Token
└── components/          # 元件樣式
```

---

### Phase 7：元件開發

**讀取模板：** `webdev/tpl/comp/SKILL.md`

**建立順序：**

1. 基礎元件（按鈕、輸入框、卡片）
2. 佈局元件（容器、網格、間距）
3. 導航元件（頁首、側邊欄）
4. 功能元件（表單、Modal）

---

### Phase 8：頁面開發

**讀取模板：** `webdev/tpl/page/SKILL.md`

**建立順序：**

1. 佈局檔案（Layout）
2. 首頁
3. 主要頁面
4. 次要頁面

---

### Phase 9：後端開發（如需要）

**讀取對應框架指南：**

- `webdev/be/laravel.md`
- `webdev/be/node.md`
- `webdev/be/django.md`
- `webdev/be/fastapi.md`

**讀取 API 設計：** `webdev/api/SKILL.md`

---

### Phase 10：響應式與無障礙

**讀取：**

- `webdev/rwd/SKILL.md`
- `webdev/ui/a11y.md`

**檢查：**

- [ ] 所有斷點測試
- [ ] 鍵盤導航
- [ ] 色彩對比度
- [ ] alt 文字

---

### Phase 11：上線前檢查

**讀取並執行：**

- `webdev/check/launch.md`
- `webdev/check/seo.md`
- `webdev/sec/SKILL.md`

---

### Phase 12：部署

**讀取：** `webdev/flow/SKILL.md`

---

## 🎯 簡化版流程（快速開發）

如果用戶明確知道需求，可以跳過精靈，直接說：

```text
用 Next.js + Tailwind + 極簡風格 建立一個作品集網站，包含首頁、作品頁、關於頁
```

AI 會：

1. 直接讀取對應的 skills 文件
2. 跳過問答
3. 直接開始開發

---

## ⚠️ 重要提醒

每次開發都必須遵守：

1. **必讀 `no-ai-feel.md`**：確保設計有人味
2. **不使用庫存照片**：使用真實照片或獨特插畫
3. **不使用空洞文案**：使用真實、有意義的內容
4. **不使用完美對稱**：增加適度的變化
5. **檢查商業感**：移除過度促銷的元素
