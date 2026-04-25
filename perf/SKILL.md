---
id: web_performance
name: web_performance
description: Web 效能與 Core Web Vitals 優化（LCP/INP/CLS、Lighthouse、bundle）。不含一般框架寫法
---

# ⚡ 效能與 Core Web Vitals

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 規則摘要 + 預算（≤600 tokens）|
| L2 | [cwv.md](cwv.md) | LCP/INP/CLS 規則與修補 |
| L2 | [lighthouse.md](lighthouse.md) | CI 整合、評分閥值 |
| L2 | [bundle.md](bundle.md) | code splitting、tree shaking |

## 觸發關鍵字

效能、慢、卡、Lighthouse、Core Web Vitals、LCP、INP、CLS、TBT、bundle、size、優化、performance

## 不觸發

一般 React/Vue 寫法 → 走 `fe/`。

## Verification

見 [`../_shared/verification-commands.md`](../_shared/verification-commands.md) 的 *Audit* 段。最低標準：

- LCP ≤ 2.5s（行動）
- INP ≤ 200ms
- CLS ≤ 0.1
- Lighthouse Performance ≥ 90
