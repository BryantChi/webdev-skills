---
id: component_composition_slot
name: component_composition_slot
description: Slot / asChild 模式（Radix / shadcn 風格）
---

# Slot / asChild 模式

## 核心問題

元件內層元素寫死，使用者想換成 `<a>` 或 `<Link>` 怎麼辦？

```tsx
// 寫死成 <button>
function Button({ children }) {
  return <button className="...">{children}</button>;
}

// 想要 <Link href> 但保留 Button 樣式 → 寫不出來
```

## Solution：asChild + Slot

Radix UI 引入的模式，shadcn/ui 全面採用：

```tsx
<Button asChild>
  <Link href="/about">關於</Link>
</Button>
```

當 `asChild={true}`，Button 不渲染自己的 element，而是把 props 與樣式**合併到 children 的根 element 上**。

## 實作（用 @radix-ui/react-slot）

```tsx
import { Slot } from '@radix-ui/react-slot';
import { cva, VariantProps } from 'class-variance-authority';

const button = cva('inline-flex items-center rounded px-4 py-2', {
  variants: {
    variant: { primary: 'bg-blue-600 text-white', ghost: 'hover:bg-gray-100' },
    size: { sm: 'text-sm', md: 'text-base', lg: 'text-lg' },
  },
  defaultVariants: { variant: 'primary', size: 'md' },
});

type Props = React.ButtonHTMLAttributes<HTMLButtonElement> &
  VariantProps<typeof button> & { asChild?: boolean };

export function Button({ className, variant, size, asChild, ...props }: Props) {
  const Comp = asChild ? Slot : 'button';
  return <Comp className={button({ variant, size, className })} {...props} />;
}
```

## 使用情境

```tsx
// 一般按鈕
<Button onClick={...}>送出</Button>

// 連結按鈕
<Button asChild>
  <Link href="/about">關於</Link>
</Button>

// 第三方元件
<Button asChild>
  <Tooltip.Trigger>?</Tooltip.Trigger>
</Button>
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| SL1 | children 必須是**單一**有效 element | Slot 只合併到一個 root |
| SL2 | 使用者要把 ref 傳下去 | `forwardRef` 或 React 19 ref-as-prop |
| SL3 | className 用 merge 工具（cn / clsx + tailwind-merge） | 避免衝突 |
| SL4 | event handler 同名要 chain | Slot 自動合併同名事件 |

## Vue 3：透過具名 slot + `<component :is>`

```vue
<script setup>
defineProps(['as']);
</script>
<template>
  <component :is="as || 'button'" class="...">
    <slot />
  </component>
</template>
```

```vue
<MyButton as="a" href="/x">關於</MyButton>
```

## 與 Compound 的搭配

shadcn/ui 範例：

```tsx
<DropdownMenu>
  <DropdownMenu.Trigger asChild>
    <Button variant="outline">選單</Button>
  </DropdownMenu.Trigger>
  <DropdownMenu.Content>
    <DropdownMenu.Item asChild><Link href="/profile">個人</Link></DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu>
```

## Anti-Pattern

| Bad | Good |
|---|---|
| `<Button as="a" href="...">` 自家發明 | `<Button asChild><a href>...</a></Button>` |
| children 多個 element | 用 fragment 包成單一 root |
| 自寫合併 className | 用 `tailwind-merge` |
