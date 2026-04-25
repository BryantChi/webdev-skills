---
id: coding_style
name: coding_style
description: JavaScript、TypeScript、PHP、Python、CSS 代碼規範
---

# 📝 Coding Style 技能

## 規範索引

| 語言 | 規範 | 說明 |
|-----|------|-----|
| JavaScript | [Airbnb](js-airbnb.md) | 最流行、嚴格 |
| TypeScript | [TS 規範](ts.md) | TypeScript 特定規範（含 strict 設定）|
| PHP | [PSR-12](php-psr12.md) | PHP 官方標準（Laravel 採用） |
| Python | [PEP8](py-pep8.md) | Python 官方標準 |
| CSS | [BEM](css-bem.md) | 命名規範 |

> **框架特定規則**：見 [`../fe/react-rules.critical.md`](../fe/react-rules.critical.md)、[`vue-rules.critical.md`](../fe/vue-rules.critical.md)、[`next-rules.critical.md`](../fe/next-rules.critical.md)。

---

## 共通原則

### 命名規範

| 類型 | JavaScript | PHP | Python |
|-----|-----------|-----|--------|
| 變數 | camelCase | $camelCase | snake_case |
| 函式 | camelCase | camelCase | snake_case |
| 類別 | PascalCase | PascalCase | PascalCase |
| 常數 | UPPER_SNAKE | UPPER_SNAKE | UPPER_SNAKE |

### 格式化

| 項目 | 建議 |
|-----|------|
| 縮排 | 2 或 4 空格（依語言）|
| 行寬 | 80-120 字元 |
| 檔案結尾 | 保留空行 |
| 引號 | 統一使用單引號或雙引號 |

---

## 工具設定

### ESLint + Prettier (JS/TS)

```json
// .eslintrc.json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser"
}
```

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

### EditorConfig

```ini
# .editorconfig
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.{php,py}]
indent_size = 4
```

---

## Git Hooks

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.css": ["stylelint --fix"]
  }
}
```

---

## 選擇精靈

不確定用什麼規範？使用 [Coding Style 精靈](../wizard/code.md)

---

## 相關技能

- [優先級制度](../_shared/priority-legend.md)（CRITICAL/HIGH/MEDIUM/LOW）
- [通用反模式](../_shared/anti-patterns.md)（資料流、DOM、樣式、表單、後端、a11y）
- [驗證指令](../_shared/verification-commands.md)（lint、test、build、audit）
- [React 規則庫](../fe/react-rules.critical.md) ｜ [optional](../fe/react-rules.optional.md)
- [Vue 規則庫](../fe/vue-rules.critical.md) ｜ [optional](../fe/vue-rules.optional.md)
- [Next 規則庫](../fe/next-rules.critical.md) ｜ [optional](../fe/next-rules.optional.md)
- [元件組合模式](../comp/SKILL.md)
- [測試體系](../test/SKILL.md)（Code Review 配套）

> **建議流程**：PR 必跑 lint + typecheck + test（見 [`../deploy/ci-github.md`](../deploy/ci-github.md)）。Code Review 時載入對應框架的 `*-rules.optional.md` 檔。
