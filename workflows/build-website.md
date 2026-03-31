---
id: build_website_workflow
name: Build Website Workflow
description: 使用 Webdev Skills 系統建立完整網站的標準工作流程
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

→ 根據回答，讀取 `spec.md` 填寫需求規格書

---

### Phase 1.5：設計推理 (關鍵步驟)

**必須執行：** 讀取並執行 `logic/reasoning.md`

**AI 動作：**
1. 分析 Phase 1 的需求。
2. 輸出 **Design Reasoning Block** (ASCII 表格)。
3. 確定風格、配色、字體與反模式。

---

### Phase 2 ~ 4：技術與詳細設定 (智慧自動化)

**基於推理結果自動決策：**
- 除非用戶有特定要求，否則跳過問答精靈。
- 直接根據需求推薦最佳技術棧 (e.g., SEO重 -> Next.js; 後台重 -> Laravel)。
- 自動填寫 `spec.md`。

*(若用戶顯式要求手動選擇，則執行對應的 `wizard/*.md`)*

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

- 讀取對應的 `code/*.md` 設定 linting
- 建立 `.editorconfig`
- 建立 `README.md`

---

### Phase 6：設計系統建立 (Design System Gen)

**必須執行：** 讀取 `logic/design-system-gen.md`

**執行動作：**
1. 根據 Reasoning Block 的結果。
2. 生成 `styles/design-system/MASTER.md`。
3. 確保包含：色彩變數、字體定義、間距系統、陰影效果。
4. **過濾反模式**：確保不包含被禁止的設計元素。

**建立 CSS/Config：**
- `tailwind.config.js` (或 `globals.css`) 必須引用 `MASTER.md` 的變數。


---

### Phase 7：元件開發

**讀取模板：** `tpl/comp/SKILL.md`

**建立順序：**

1. 基礎元件（按鈕、輸入框、卡片）
2. 佈局元件（容器、網格、間距）
3. 導航元件（頁首、側邊欄）
4. 功能元件（表單、Modal）

---

### Phase 8：頁面開發

**讀取模板：** `tpl/page/SKILL.md`

**建立順序：**

1. 佈局檔案（Layout）
2. 首頁
3. 主要頁面
4. 次要頁面

---

### Phase 9：後端開發（如需要）

**讀取對應框架指南：**

- `be/laravel.md`
- `be/node.md`
- `be/django.md`
- `be/fastapi.md`

**讀取 API 設計：** `api/SKILL.md`

---

### Phase 10：響應式與無障礙

**讀取：**

- `rwd/SKILL.md`
- `ui/a11y.md`

**檢查：**

- [ ] 所有斷點測試
- [ ] 鍵盤導航
- [ ] 色彩對比度
- [ ] alt 文字

---

### Phase 11：上線前檢查

**讀取並執行：**

- `check/launch.md`
- `check/seo.md`
- `sec/SKILL.md`

---

### Phase 12：部署

**讀取：** `flow/SKILL.md`

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

1. **必讀 `ui/no-ai-feel.md`**：確保設計有人味
2. **不使用庫存照片**：使用真實照片或獨特插畫
3. **不使用空洞文案**：使用真實、有意義的內容
4. **不使用完美對稱**：增加適度的變化
5. **檢查商業感**：移除過度促銷的元素
