---
id: component_composition
name: component_composition
description: 元件組合模式（Compound Component、Slot Props、Headless UI）。取代 boolean prop 暴衝
---

# 🧩 元件組合模式

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 何時用何模式 |
| L2 | [compound.md](compound.md) | Compound Component |
| L2 | [slot.md](slot.md) | Slot / asChild |
| L2 | [headless.md](headless.md) | Headless UI 模式 |

## 為何重要

業界共識（Vercel Composition Patterns）：**用組合取代 boolean prop**。否則元件 API 會爆炸：

```tsx
// ❌ Boolean 暴衝
<Button primary large rounded loading icon iconRight outline />

// ✅ 用組合
<Button.Root size="lg" variant="primary">
  <Button.Icon><LoaderIcon /></Button.Icon>
  <Button.Label>送出</Button.Label>
</Button.Root>
```

## 觸發關鍵字

元件設計、API 設計、compound、slot、headless、asChild、shadcn 模式

## Verification

- 元件 props 是否 ≤ 5 個？超過考慮拆 children
- 是否能不改元件而擴充行為？（用 children/slot）
