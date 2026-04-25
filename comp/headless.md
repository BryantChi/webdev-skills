---
id: component_composition_headless
name: component_composition_headless
description: Headless UI 模式（行為與樣式徹底分離）
---

# Headless UI 模式

## 核心理念

**邏輯由 hook / 物件提供，DOM 與樣式由使用者寫**。

優勢：
- 一份邏輯、N 種視覺
- 易於 a11y（邏輯封裝在內）
- 跨設計系統共用

## 業界範例

| 函式庫 | 形式 |
|--------|------|
| TanStack Table | hook + 物件 model |
| Headless UI（Tailwind Labs） | 元件但無樣式 |
| React Aria | 純 hook |
| Downshift（autocomplete） | render-prop |
| Reach UI | 元件 + class hooks |

## 範例：Headless Disclosure（折疊面板）

```tsx
import { useState, useId, useCallback } from 'react';

export function useDisclosure(defaultOpen = false) {
  const [open, setOpen] = useState(defaultOpen);
  const triggerId = useId();
  const panelId = useId();

  const toggle = useCallback(() => setOpen((o) => !o), []);

  const triggerProps = {
    id: triggerId,
    'aria-expanded': open,
    'aria-controls': panelId,
    onClick: toggle,
    type: 'button' as const,
  };

  const panelProps = {
    id: panelId,
    role: 'region',
    'aria-labelledby': triggerId,
    hidden: !open,
  };

  return { open, toggle, setOpen, triggerProps, panelProps };
}
```

## 使用（兩種樣式）

```tsx
// 樣式 A：Tailwind
function FaqItem({ question, answer }) {
  const { open, triggerProps, panelProps } = useDisclosure();
  return (
    <div className="border rounded">
      <button {...triggerProps} className="w-full p-4 flex justify-between">
        {question}
        <span>{open ? '−' : '+'}</span>
      </button>
      <div {...panelProps} className="p-4 prose">{answer}</div>
    </div>
  );
}

// 樣式 B：Bootstrap，邏輯一字不改
function BsFaqItem({ question, answer }) {
  const { open, triggerProps, panelProps } = useDisclosure();
  return (
    <div className="card">
      <button {...triggerProps} className="card-header btn-link">{question}</button>
      <div {...panelProps} className="card-body">{answer}</div>
    </div>
  );
}
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| H1 | 提供完整 a11y props | 邏輯內封 aria-*, role |
| H2 | hook 回傳 props 物件，由使用者展開 | 不寫 DOM |
| H3 | 鍵盤導航邏輯內封 | 例如 ArrowDown 切下個選項 |
| H4 | 不依賴特定樣式系統 | 純行為 |

## 何時用 Headless（vs Compound + Slot）

| 場景 | 選 |
|------|------|
| 樣式變化大（多品牌） | Headless |
| 邏輯極複雜（table、虛擬列表） | Headless |
| 一般 UI（按鈕、Dialog） | Compound + Slot |
| 內部 design system | Compound + Slot |

## 範例：用 React Aria

```tsx
import { useButton, useFocusRing } from 'react-aria';
import { useRef } from 'react';

export function Button({ children, ...props }) {
  const ref = useRef(null);
  const { buttonProps } = useButton(props, ref);
  const { isFocusVisible, focusProps } = useFocusRing();
  return (
    <button
      ref={ref}
      {...buttonProps}
      {...focusProps}
      data-focus-visible={isFocusVisible}
      className="..."
    >
      {children}
    </button>
  );
}
```

換樣式只改 className，行為不動。

## Anti-Pattern

| Bad | Good |
|---|---|
| Hook 內 inject className | hook 只回 props，不管樣式 |
| 把 a11y 留給使用者 | 內封好 aria-* |
| 用 render-prop 又包一層 | hook 形式更乾淨 |
