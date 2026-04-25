---
id: priority_legend
name: priority_legend
description: 跨模組共用的規則優先級定義，所有 rules 檔案請引用此檔
---

# 規則優先級制度

| Level | 含義 | 違反代價 | 範例 |
|-------|------|---------|------|
| **CRITICAL** | 違反會導致功能、效能或安全問題 | 必修 | XSS 風險、N+1 查詢、async waterfall |
| **HIGH** | 顯著影響使用者體驗或可維護性 | 強烈建議修 | 缺少 key、未處理 loading state |
| **MEDIUM** | 提升程式碼品質 | 視時間修 | 命名慣例、過度抽象 |
| **LOW** | 風格偏好 | 可保留 | 引號風格、import 排序 |

## 預設載入規則

- 一般任務：僅載入 **CRITICAL + HIGH**（檔名 `*.critical.md`）
- Code Review / 重構：再載入 **MEDIUM + LOW**（檔名 `*.optional.md`）
- 不要在同一個檔案塞所有等級，請拆檔

## 引用方式

各模組規則檔頂部加：

```markdown
> 優先級定義見 [_shared/priority-legend.md](../_shared/priority-legend.md)
```
