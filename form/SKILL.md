---
id: web_form_system
name: web_form_system
description: 表單體系（React Hook Form + Zod、TanStack Form、server actions、檔案上傳）
---

# 📝 表單體系

> 優先級定義見 [`../_shared/priority-legend.md`](../_shared/priority-legend.md)

## 三層級導引

| 層級 | 檔案 | 用途 |
|------|------|------|
| L0 | 本檔 | 路由表 |
| L1 | [compact.md](compact.md) | 選型 + 必裝 |
| L2 | [react-hook-form.md](react-hook-form.md) | RHF + Zod（最主流） |
| L2 | [tanstack-form.md](tanstack-form.md) | TanStack Form（型別最強） |
| L2 | [server-actions-zod.md](server-actions-zod.md) | Next 15 server action |
| L2 | [file-upload.md](file-upload.md) | 檔案上傳（presigned URL） |

## 觸發關鍵字

form、表單、validation、驗證、Zod、Yup、React Hook Form、Formik、上傳、upload

## Verification

- 是否用 schema 驗證（Zod / Yup）？
- 錯誤訊息是否與欄位 `aria-describedby` 關聯？
- server 端是否再驗一次？
