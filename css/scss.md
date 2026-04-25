---
id: scss_guide
name: scss_guide
description: SCSS/Sass 開發最佳實踐
---

# 🎨 SCSS 指南

## 安裝

```bash
npm install -D sass
```

---

## 變數

```scss
// _variables.scss
$primary: #3b82f6;
$secondary: #64748b;
$font-family: 'Inter', sans-serif;
$border-radius: 8px;

// 使用
.button {
  background: $primary;
  border-radius: $border-radius;
}
```

---

## 巢狀

```scss
.card {
  padding: 20px;
  
  &__title {
    font-size: 1.25rem;
  }
  
  &__body {
    margin-top: 12px;
  }
  
  &:hover {
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  
  &--featured {
    border: 2px solid $primary;
  }
}
```

---

## Mixin

```scss
// 定義
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

@mixin button-variant($bg, $color: white) {
  background: $bg;
  color: $color;
  &:hover {
    background: darken($bg, 10%);
  }
}

// 使用
.hero {
  @include flex-center;
}

.btn-primary {
  @include button-variant($primary);
}
```

---

## 函式

```scss
@function rem($px) {
  @return #{$px / 16}rem;
}

.title {
  font-size: rem(24); // 1.5rem
}
```

---

## 檔案結構

```
scss/
├── _variables.scss
├── _mixins.scss
├── _reset.scss
├── base/
│   ├── _typography.scss
│   └── _buttons.scss
├── components/
│   ├── _card.scss
│   └── _navbar.scss
├── layouts/
│   ├── _header.scss
│   └── _footer.scss
└── main.scss
```

```scss
// main.scss
@import 'variables';
@import 'mixins';
@import 'reset';

@import 'base/typography';
@import 'base/buttons';

@import 'components/card';
@import 'components/navbar';

@import 'layouts/header';
@import 'layouts/footer';
```

---

## 最佳實踐

1. **變數統一管理**
2. **Mixin 重用樣式**
3. **7-1 模式組織檔案**
4. **BEM 命名規範**
5. **避免過深巢狀 (3層以內)**

---

## 相關技能

- [BEM 命名規範](../code/css-bem.md)
- [Tailwind 替代](tailwind.md)
- [shadcn/ui + Radix](shadcn-radix.md)
- [Bootstrap（基於 SCSS）](bootstrap.md)
- [設計系統生成](../logic/design-system-gen.md)
- [元件組合模式](../comp/SKILL.md)

> **建議**：SCSS + BEM 適合需要嚴謹 design system 的專案；現代 React/Vue 專案大多已遷移到 Tailwind + shadcn。
