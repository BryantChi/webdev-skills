---
id: design_logic
name: design_logic
description: 設計推理與生成邏輯（reasoning + design-system-gen）路由表
---

# 🧠 設計邏輯

> 本目錄為 SOP Phase 1（推理）與 Phase 2（設計系統生成）的核心檔案。

## 路由

| 階段 | 檔案 | 用途 |
|------|------|------|
| Phase 1 | [reasoning.md](reasoning.md) | 設計推理引擎（產出 Design Reasoning Block） |
| Phase 2 | [design-system-gen.md](design-system-gen.md) | Master + Overrides 設計系統生成 |

## 觸發關鍵字

設計推理、reasoning、design system、MASTER.md、設計決策、反模式過濾、token、CSS variables

## 執行流程

```
用戶需求 → reasoning.md（產出 ASCII Block）
       → design-system-gen.md（產出 styles/design-system/MASTER.md）
       → 框架配置（tailwind.config / globals.css）映射 MASTER 變數
```

## 相關技能

- [去 AI 感設計指南](../ui/no-ai-feel.md)（三段式禁用 / 替代 / 反例）
- [55+ 種設計風格](../ui/styles/index.md)
- [shadcn/ui + Radix](../css/shadcn-radix.md)（design token 強制）
- [元件組合模式](../comp/SKILL.md)
- [完整建站 15 階段](../workflows/build-website.md)

## 重要原則

| # | 規則 |
|---|------|
| L1 | 寫 code 前必先輸出 Design Reasoning Block |
| L2 | 推理結果寫入 `.agent/profile.json`，避免重複觸發 |
| L3 | 過濾反模式（律師不用霓虹色、SaaS 不用 Times New Roman） |
| L4 | 同一專案只一份 MASTER.md，頁面差異用 `pages/<name>.md` 覆寫 |
| L5 | 不要在實作期發明新 hex / 字體大小，必須對應 MASTER |
