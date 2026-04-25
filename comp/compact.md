---
id: component_composition_compact
name: component_composition_compact
description: 元件組合模式選擇指南（精煉版）
---

# 元件組合模式選擇

## 三大模式比較

| 模式 | 適用 | 範例 |
|------|------|------|
| **Compound Component** | 多個子元件協作（Tabs、Accordion、Select） | Radix Tabs |
| **Slot / asChild** | 想替換內層 DOM 元素 | Radix asChild、shadcn |
| **Headless** | 邏輯與樣式徹底分離 | TanStack Table、Headless UI |

## 何時換模式

| 訊號 | 動作 |
|------|------|
| props > 5 個 boolean | 改 compound |
| 需要 wrapper 換 element（`<a>` vs `<button>`） | 加 asChild |
| 樣式變化大、邏輯穩定 | 改 headless |
| 同元件被改造 N 次 | 改 compound + slot |

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| C1 | 不用 boolean prop 表達互斥狀態 | `<Btn primary secondary>` | `variant="primary"` |
| C2 | 不在 prop 塞 ReactNode 樹 | `header={<H/>} footer={<F/>}` | `<Card.Header/>` |
| C3 | 不寫死內層元素 | 元件內 `<a>` 寫死 | `asChild` 或 `as="a"` |
| C4 | 用 context 傳 compound 狀態 | 經多層 prop drill | `useContext(TabsCtx)` |
| C5 | TypeScript 強類型 children | `children: ReactNode` | `children: ReactElement<typeof Tab>` |

## 常見決策樹

```
有狀態（open/active/selected）？
├─ 是 → Compound (Tabs/Accordion/Select)
└─ 否 → 單一元件 + slot
        ↓
        需要替換外層元素？
        ├─ 是 → asChild
        └─ 否 → 簡單 props 即可

樣式很多變？
└─ 是 → Headless（行為從 hook，樣式由使用者）
```

## 業界範例

| 函式庫 | 模式 |
|--------|------|
| Radix UI | Compound + asChild |
| shadcn/ui | 在 Radix 上加樣式 |
| Headless UI（Tailwind Labs） | Compound |
| TanStack Table | Headless |
| React Aria | Hook-based headless |
| MUI base | Slot |
