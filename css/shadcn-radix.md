---
id: css_shadcn_radix
name: css_shadcn_radix
description: shadcn/ui CLI v4 + Radix primitives + design token 實踐
---

# shadcn/ui + Radix

> 業界 2026 主流：Tailwind v4 + Radix primitives + shadcn 工作流。設計理念見 [`../ui/no-ai-feel.md`](../ui/no-ai-feel.md)，組合模式見 [`../comp/SKILL.md`](../comp/SKILL.md)。

## 為何選 shadcn 而非「元件庫」

| 點 | shadcn | MUI / Antd |
|---|---|---|
| 程式碼歸屬 | 複製進你的 repo，可改 | npm 套件，難改 |
| Bundle | 只含你用的 | 整包 |
| 設計 token | CSS variables，由你定 | 套件主題系統 |
| a11y | Radix 內建 | 不一定 |
| 升級 | 各元件獨立 | 整包升級風險高 |

## 初始化

```bash
npx shadcn@latest init
# 回答：style (default/new-york)、base color、CSS variables (Yes)
npx shadcn@latest add button card dialog
```

產出：
- `components/ui/*.tsx` — 元件原始碼（你的）
- `components.json` — config
- `app/globals.css` — design tokens（CSS vars）
- `lib/utils.ts` — `cn()` className merge

## components.json 重點

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "app/globals.css",
    "baseColor": "neutral",
    "cssVariables": true
  },
  "aliases": {
    "components": "@/components",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "utils": "@/lib/utils"
  }
}
```

> CLI v4（2026/03）有 `npx shadcn info --json`，AI 工具可讀取你的設定。

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| SH1 | 永遠用 design token，不寫死 hex | `bg-[#3b82f6]` | `bg-primary` |
| SH2 | 改樣式直接編元件原始碼，不要 wrap 一層 | `<MyButton><Button.../></MyButton>` | 直接改 `components/ui/button.tsx` |
| SH3 | 互動元件用 Radix（Dialog/Menu/Tooltip） | 自寫 a11y | shadcn 已封 |
| SH4 | className 用 `cn()`（合併 + tw-merge） | 字串拼接 | `cn(base, conditional && extra)` |
| SH5 | variant 用 `class-variance-authority`（cva） | switch case 拼 class | cva |
| SH6 | dark mode 用 CSS vars + `.dark` class | 重寫所有顏色 | tokens 自動切 |

## Design Token 範例（globals.css）

```css
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 240 10% 4%;
    --primary: 240 5.9% 10%;
    --primary-foreground: 0 0% 98%;
    --muted: 240 4.8% 95.9%;
    --muted-foreground: 240 3.8% 46.1%;
    --border: 240 5.9% 90%;
    --radius: 0.5rem;
  }
  .dark {
    --background: 240 10% 4%;
    --foreground: 0 0% 98%;
    /* ... */
  }
}
```

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
      },
      borderRadius: { lg: 'var(--radius)', md: 'calc(var(--radius) - 2px)' },
    },
  },
};
```

## Radix 與 asChild（搭配）

```tsx
// components/ui/dropdown-menu.tsx 已導出
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="outline">選單</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem asChild>
      <Link href="/profile">個人</Link>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

## 升級 / 同步

```bash
npx shadcn@latest diff button     # 看官方版本與你的差異
npx shadcn@latest add button --overwrite  # 強制覆寫（小心）
```

通常**不要全覆寫**，挑 diff 你想要的 patch。

## Anti-Pattern（去 AI 感對齊）

| Bad | Good |
|---|---|
| 全部用 default tokens 不改 | 改 `--primary` 反映品牌 |
| 整站只有 shadcn 預設配色 | 加品牌色 + 反差強調色 |
| 用 shadcn 卻又混 MUI/AntD | 一個體系到底 |
| 元件只用而不改原始碼 | shadcn 設計就是讓你改 |

## 與 styleseed 等業界資源

| 來源 | 學什麼 |
|------|------|
| [shadcn/ui 官方](https://ui.shadcn.com) | 元件、blocks |
| [styleseed](https://github.com/bitjaru/styleseed) | 5 種品牌風格（Toss/Stripe/Linear/Vercel/Notion）token 實作 |
| [tweakcn](https://tweakcn.com) | 線上調 token 主題 |
| Radix Colors | 12 階配色系統 |

## 驗證

| 工具 | 抓什麼 |
|------|------|
| `npx shadcn info --json` | 確認 config |
| TypeScript | 元件 props 型別 |
| axe-core | a11y（Radix 已內建大多） |
