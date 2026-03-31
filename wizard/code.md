---
id: coding_style_wizard
name: coding_style_wizard
description: 透過 15 個問題引導選擇最適合的程式碼規範和工具設定
---

# 📝 Coding Style 選擇精靈

> AI 請依序詢問以下問題，根據回答推薦 Coding Style 和工具設定

---

## 階段 1/5：程式語言

### Q1. 使用的程式語言（可複選）

**前端：**
| 語言 | 主要規範選項 |
|-----|-------------|
| □ JavaScript (ES6+) | Airbnb / Google / Standard |
| □ TypeScript | TypeScript ESLint / Airbnb |
| □ HTML | HTMLHint |
| □ CSS/SCSS | Stylelint / BEM / SMACSS |

**後端：**
| 語言 | 主要規範選項 |
|-----|-------------|
| □ PHP | PSR-12 / Laravel |
| □ Python | PEP 8 / Google / Black |
| □ Node.js | 同 JavaScript |
| □ Go | gofmt (官方) |
| □ Ruby | RuboCop |

**其他：**
| 語言 | 主要規範選項 |
|-----|-------------|
| □ SQL | SQL Style Guide |
| □ Shell Script | ShellCheck |
| □ Markdown | markdownlint |

---

## 階段 2/5：專案性質與團隊

### Q2. 專案類型

| 選項 | 說明 | 規範建議 |
|-----|------|---------|
| A) 👤 個人專案 | 自己維護 | 可較彈性，但建議基本規範 |
| B) 👥 團隊專案 | 多人協作 | 必須統一規範 |
| C) 🏢 企業專案 | 公司內部 | 遵循公司規範或業界標準 |
| D) 🌍 開源專案 | 社群貢獻 | 選擇廣為接受的規範 |

### Q3. 團隊經驗

| 選項 | 說明 | 建議 |
|-----|------|-----|
| A) 👶 大多新手 | 經驗 < 1年 | 規則簡單、有清楚說明 |
| B) 👨‍💻 混合程度 | 新手+資深 | 規則明確、文件完善 |
| C) 👨‍🔬 大多資深 | 經驗 > 3年 | 可用嚴格規範 |
| D) 🤷 不確定 | - | 推薦中等嚴格度 |

### Q4. 現有規範

| 選項 | 說明 |
|-----|------|
| A) 有現成規範：_______ | 需延續 |
| B) 有部分規範 | 需補充 |
| C) 完全沒有 | 從頭建立 |
| D) 有但想更換 | 需遷移策略 |

---

## 階段 3/5：規範偏好

### Q5. 嚴格程度

請選擇 1-5 的嚴格度：

```
寬鬆 ◀━━━━━━━━━━━━━━━━━━━━━━━━━━━▶ 嚴格
      1      2      3      4      5
```

| 等級 | 說明 | 規則數量 |
|-----|------|---------|
| 1 | 只管基本格式 | ~20 條 |
| 2 | 加上命名規範 | ~40 條 |
| 3 | 加上最佳實踐 | ~80 條 |
| 4 | 嚴格型別+結構 | ~150 條 |
| 5 | 企業級嚴格 | ~200+ 條 |

### Q6. 縮排偏好

| 選項 | 適用情境 |
|-----|---------|
| A) 2 空格 | JS/TS 社群主流 |
| B) 4 空格 | Python/PHP 主流 |
| C) Tab | 部分團隊偏好 |
| D) 依語言慣例 | 自動選擇 |

### Q7. 引號偏好（JavaScript/TypeScript）

| 選項 | 規範 |
|-----|------|
| A) 'single' 單引號 | Airbnb、Standard |
| B) "double" 雙引號 | Google |
| C) 無偏好 | 依選擇規範 |

### Q8. 分號使用（JavaScript/TypeScript）

| 選項 | 規範 |
|-----|------|
| A) 總是使用 | Airbnb、Google |
| B) 省略分號 | Standard |
| C) 無偏好 | 依選擇規範 |

### Q9. 函式風格（JavaScript/TypeScript）

| 選項 | 說明 |
|-----|------|
| A) function | 傳統函式宣告 |
| B) () => {} | 箭頭函式 |
| C) 視情況 | 方法用 function，callback 用箭頭 |
| D) 無偏好 | 依選擇規範 |

---

## 階段 4/5：工具整合

### Q10. 使用工具（可複選）

**JavaScript/TypeScript：**
| 工具 | 用途 |
|-----|------|
| □ ESLint | 程式碼檢查 |
| □ Prettier | 程式碼格式化 |
| □ TypeScript | 型別檢查 |

**CSS：**
| 工具 | 用途 |
|-----|------|
| □ Stylelint | CSS 檢查 |
| □ PostCSS | CSS 處理 |

**PHP：**
| 工具 | 用途 |
|-----|------|
| □ PHP_CodeSniffer | 程式碼檢查 |
| □ PHP-CS-Fixer | 自動修復 |
| □ PHPStan/Psalm | 靜態分析 |

**Python：**
| 工具 | 用途 |
|-----|------|
| □ Pylint | 程式碼檢查 |
| □ Flake8 | 格式檢查 |
| □ Black | 自動格式化 |
| □ mypy | 型別檢查 |

**通用：**
| 工具 | 用途 |
|-----|------|
| □ Husky | Git Hooks |
| □ lint-staged | 只檢查修改檔案 |
| □ commitlint | Commit 訊息規範 |
| □ EditorConfig | 編輯器統一設定 |

### Q11. 檢查時機（可複選）

| 時機 | 說明 |
|-----|------|
| □ 儲存時 | 即時反饋 |
| □ Commit 前 | 防止問題進入版控 |
| □ Push 前 | 最後防線 |
| □ PR/MR 時 | CI 檢查 |
| □ 手動執行 | 需要時才檢查 |

### Q12. 開發環境

| 選項 | 需要的設定檔 |
|-----|-------------|
| A) VS Code | .vscode/settings.json |
| B) WebStorm/PHPStorm | .idea/ 設定 |
| C) Vim/Neovim | vim 設定 |
| D) Sublime Text | Sublime 設定 |
| E) 其他 | 通用設定 |

---

## 階段 5/5：進階設定

### Q13. 命名規範

**變數/函式：**
| 選項 | 範例 |
|-----|------|
| A) camelCase | getUserName |
| B) snake_case | get_user_name |
| C) 依語言慣例 | JS: camelCase, Python: snake_case |

**常數：**
| 選項 | 範例 |
|-----|------|
| A) UPPER_SNAKE_CASE | MAX_COUNT |
| B) 與變數相同 | maxCount |

**類別/元件：**
| 選項 | 範例 |
|-----|------|
| A) PascalCase | UserProfile |
| B) 依框架慣例 | - |

**檔案名稱：**
| 選項 | 範例 |
|-----|------|
| A) kebab-case | user-profile.js |
| B) camelCase | userProfile.js |
| C) PascalCase | UserProfile.js |
| D) snake_case | user_profile.js |

### Q14. 註解規範

| 選項 | 說明 |
|-----|------|
| A) 完整文件註解 | JSDoc/PHPDoc 所有公開函式 |
| B) 關鍵部分註解 | 複雜邏輯、API |
| C) 最少註解 | 程式碼自解釋 |
| D) 無特別要求 | - |

### Q15. 其他偏好（可複選）

| 項目 | 說明 |
|-----|------|
| □ 強制 TypeScript strict | 嚴格型別模式 |
| □ 禁止 any 類型 | 完全型別覆蓋 |
| □ 強制 const | 優先使用 const |
| □ 禁止 var | 使用 let/const |
| □ 要求解構賦值 | 優先解構 |
| □ 要求可選鏈 | obj?.prop |
| □ 要求空值合併 | ?? 運算子 |
| □ 最大行寬限制 | 80/100/120 字元 |
| □ 禁止 console.log | 生產環境 |
| □ 要求錯誤處理 | try-catch |

---

## 規範推薦邏輯

### JavaScript/TypeScript 規範比較

| 規範 | 嚴格度 | 引號 | 分號 | 特色 |
|-----|-------|-----|------|-----|
| Airbnb | 高 | 單引號 | 要 | 最流行、最嚴格 |
| Google | 中高 | 雙引號 | 要 | Google 風格 |
| Standard | 中 | 單引號 | 不要 | 簡潔、無分號 |
| XO | 高 | 單引號 | 要 | 類似 Airbnb |

### PHP 規範比較

| 規範 | 嚴格度 | 特色 |
|-----|-------|-----|
| PSR-12 | 中高 | PHP FIG 官方 |
| Laravel | 中 | Laravel 風格 |

### Python 規範比較

| 規範 | 嚴格度 | 特色 |
|-----|-------|-----|
| PEP 8 | 中 | Python 官方 |
| Google | 中高 | Google 風格 |
| Black | 高 | 無爭議格式化 |

### CSS 規範比較

| 規範 | 類型 | 特色 |
|-----|------|-----|
| BEM | 命名 | 區塊-元素-修飾符 |
| SMACSS | 架構 | 分類組織 |
| ITCSS | 架構 | 倒三角形分層 |

---

## 輸出格式

```
╔══════════════════════════════════════════════════════════════╗
║                   ⚙️ Coding Style 推薦結果                    ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  📜 JavaScript/TypeScript                                    ║
║  ┌────────────────────────────────────────────────────────┐  ║
║  │ 推薦規範：Airbnb + TypeScript                          │  ║
║  │                                                        │  ║
║  │ 主要規則：                                             │  ║
║  │ • 縮排：2 空格                                         │  ║
║  │ • 引號：單引號                                         │  ║
║  │ • 分號：需要                                           │  ║
║  │ • 行寬：100 字元                                       │  ║
║  │ • 尾逗號：多行時需要                                   │  ║
║  │ • 命名：camelCase（變數）、PascalCase（類別）          │  ║
║  └────────────────────────────────────────────────────────┘  ║
║                                                              ║
║  📜 CSS                                                      ║
║  ┌────────────────────────────────────────────────────────┐  ║
║  │ 推薦規範：Stylelint + BEM                              │  ║
║  │                                                        │  ║
║  │ 命名規則：                                             │  ║
║  │ • 區塊：.block                                         │  ║
║  │ • 元素：.block__element                                │  ║
║  │ • 修飾符：.block--modifier                             │  ║
║  └────────────────────────────────────────────────────────┘  ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  🔧 設定檔生成                                               ║
║                                                              ║
║  以下設定檔已準備好：                                         ║
║                                                              ║
║  ✓ .eslintrc.js                                             ║
║  ✓ .prettierrc                                              ║
║  ✓ .stylelintrc.json                                        ║
║  ✓ .editorconfig                                            ║
║  ✓ tsconfig.json                                            ║
║  ✓ .husky/pre-commit                                        ║
║  ✓ .lintstagedrc                                            ║
║  ✓ .vscode/settings.json                                    ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  📦 需要安裝的套件                                           ║
║                                                              ║
║  npm install -D \                                            ║
║    eslint \                                                  ║
║    @typescript-eslint/parser \                              ║
║    @typescript-eslint/eslint-plugin \                       ║
║    eslint-config-airbnb-typescript \                        ║
║    prettier \                                                ║
║    eslint-config-prettier \                                  ║
║    stylelint \                                               ║
║    husky \                                                   ║
║    lint-staged                                               ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║  🔗 相關資源                                                 ║
║  • Airbnb 規範詳細：[js-airbnb.md](../code/js-airbnb.md)    ║
║  • BEM 規範詳細：[css-bem.md](../code/css-bem.md)           ║
╚══════════════════════════════════════════════════════════════╝
```

---

## 常見組合推薦

### React + TypeScript 專案
```
ESLint: airbnb-typescript
Prettier: 預設 + 單引號
Stylelint: standard
Git Hooks: husky + lint-staged
```

### Vue + TypeScript 專案
```
ESLint: @vue/eslint-config-typescript
Prettier: 預設 + 單引號
Stylelint: standard
Git Hooks: husky + lint-staged
```

### Laravel 專案
```
PHP: PSR-12 + Laravel Pint
JS: Standard (或 Airbnb)
CSS: BEM
Git Hooks: husky + lint-staged
```

### Python 專案
```
Linter: Pylint + Flake8
Formatter: Black
Type: mypy
Pre-commit: pre-commit hooks
```
