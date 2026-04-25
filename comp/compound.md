---
id: component_composition_compound
name: component_composition_compound
description: Compound Component 完整指南
---

# Compound Component

## 核心概念

把元件拆成多個小元件，**用 React Context 共享狀態**，使用者用組合方式排版。

## 完整範例：Tabs

```tsx
import { createContext, useContext, useState } from 'react';

type TabsCtx = { active: string; setActive: (v: string) => void };
const Ctx = createContext<TabsCtx | null>(null);

function useTabs() {
  const ctx = useContext(Ctx);
  if (!ctx) throw new Error('Tabs.* must be inside <Tabs>');
  return ctx;
}

export function Tabs({ defaultValue, children }: { defaultValue: string; children: React.ReactNode }) {
  const [active, setActive] = useState(defaultValue);
  return <Ctx.Provider value={{ active, setActive }}>{children}</Ctx.Provider>;
}

Tabs.List = function List({ children }: { children: React.ReactNode }) {
  return <div role="tablist" className="flex gap-2">{children}</div>;
};

Tabs.Trigger = function Trigger({ value, children }: { value: string; children: React.ReactNode }) {
  const { active, setActive } = useTabs();
  const isActive = active === value;
  return (
    <button
      role="tab"
      aria-selected={isActive}
      onClick={() => setActive(value)}
      data-state={isActive ? 'active' : 'inactive'}
    >
      {children}
    </button>
  );
};

Tabs.Panel = function Panel({ value, children }: { value: string; children: React.ReactNode }) {
  const { active } = useTabs();
  if (active !== value) return null;
  return <div role="tabpanel">{children}</div>;
};
```

## 使用

```tsx
<Tabs defaultValue="overview">
  <Tabs.List>
    <Tabs.Trigger value="overview">概覽</Tabs.Trigger>
    <Tabs.Trigger value="settings">設定</Tabs.Trigger>
  </Tabs.List>
  <Tabs.Panel value="overview">...</Tabs.Panel>
  <Tabs.Panel value="settings">...</Tabs.Panel>
</Tabs>
```

## 為何不直接用 props 陣列

```tsx
// ❌ 不靈活
<Tabs items={[
  { value: 'overview', label: '概覽', content: <... /> },
  { value: 'settings', label: '設定', content: <... /> },
]} />
```

問題：
- 無法在 trigger 與 panel 中插入額外內容
- 樣式覆寫困難
- 不能根據 active 條件渲染複雜結構

## CRITICAL 規則

| # | Rule | 範例 |
|---|---|---|
| CC1 | Context 加錯誤檢查 | `if (!ctx) throw new Error()` |
| CC2 | a11y attributes 必加 | `role`, `aria-selected`, `tabindex` |
| CC3 | Trigger 用 `<button>`，不用 `<div>` | 鍵盤可達 |
| CC4 | 鍵盤導航：方向鍵切換 | `onKeyDown` 處理 ArrowLeft/Right |
| CC5 | 子元件名空間化 | `Tabs.List`, `Tabs.Trigger`（避免命名衝突） |

## Vue 3 等價作法

```vue
<!-- Tabs.vue -->
<script setup>
import { provide, ref } from 'vue';
const props = defineProps(['defaultValue']);
const active = ref(props.defaultValue);
provide('tabs', { active, setActive: (v) => (active.value = v) });
</script>
<template><div><slot /></div></template>
```

```vue
<!-- TabsTrigger.vue -->
<script setup>
import { inject, computed } from 'vue';
const { active, setActive } = inject('tabs');
const props = defineProps(['value']);
const isActive = computed(() => active.value === props.value);
</script>
<template>
  <button role="tab" :aria-selected="isActive" @click="setActive(value)"><slot /></button>
</template>
```

## 何時不用 Compound

- 元件只有一層、無互動 → 簡單 props 即可
- 元件邏輯極複雜、樣式超多變 → 改 Headless（見 [headless.md](headless.md)）
