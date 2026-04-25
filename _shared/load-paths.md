---
id: load_paths
name: load_paths
description: 任務類型 → 最小載入檔案集對照表。AI 應依此載入，避免「以防萬一」全載
---

# 載入路徑表

> 原則：**只載當下需要的最小檔案集**。L1 = compact 摘要、L2 = 詳細內容。

## 任務 → 最小載入

| 任務類型 | L1 必要 | L2 視需求 |
|---|---|---|
| 建立網站（從零） | `wizard/tech.md` + `wizard/style.md` + `workflows/build-website.md` | 對應風格、框架詳檔 |
| 改色 / 微調 UI | `ui/color.md` 或 `ui/typo.md` | `ui/styles/<選定風格>.md` |
| 開發新元件 | `comp/compact.md` + `tpl/comp/SKILL.md` | `comp/{compound,slot,headless}.md` |
| 效能優化 | `perf/compact.md` + `check/lighthouse-checklist.md` | `perf/{cwv,lighthouse,bundle}.md` |
| 寫測試（E2E） | `test/compact.md` + `test/e2e-playwright.md` | `test/visual-regression.md` |
| 寫測試（單元） | `test/compact.md` + `test/unit-vitest.md` | — |
| a11y 稽核 | `test/a11y-axe.md` + `check/a11y-checklist.md` | `ui/a11y.md` |
| SEO 設定 | `seo/compact.md` + `check/seo-checklist.md` | `seo/{schema,sitemap-robots}.md` |
| Code Review（React） | `fe/react-rules.critical.md` + `code/SKILL.md` | `fe/react-rules.optional.md` |
| Code Review（Vue） | `fe/vue-rules.critical.md` + `code/SKILL.md` | `fe/vue-rules.optional.md` |
| 重構 / 抽元件 | `comp/compact.md` + `fe/<框架>-rules.optional.md` | — |
| 升級 shadcn/ui | `css/shadcn-radix.md` | `css/tailwind.md` |
| Bug Fix | 視 bug 類別載入對應 compact | 對應詳檔 |
| 安全稽核 | `sec/SKILL.md` + `_shared/anti-patterns.md` | `check/launch.md` |
| 加多語系 | `i18n/compact.md` + 對應框架檔 | `seo/canonical.md`（hreflang） |
| 部署設定 | `deploy/compact.md` + 對應平台檔 | `deploy/ci-github.md` |
| 加錯誤監控 | `obs/sentry.md` | `obs/logging.md` |
| 加 Web Vitals 上報 | `obs/web-vitals-reporter.md` | `perf/cwv.md` |
| 加狀態管理 | `state/compact.md` + 對應套件檔 | `_shared/anti-patterns.md` 資料流段 |
| 寫表單（含驗證） | `form/compact.md` + `form/react-hook-form.md` | `form/server-actions-zod.md`、`form/file-upload.md` |
| 加認證系統 | `auth/compact.md` + 對應方案檔 | `auth/jwt-cookie.md`、`auth/rbac-abac.md` |
| 加動畫 | `anim/compact.md` + 對應套件檔 | `anim/view-transitions.md`、`ui/no-ai-feel.md` |
| 完整建站（15 階段） | `workflows/build-website.md` + 三個 wizard | 各 phase 對應 compact |

## 強制規則

1. **不要為了「以防萬一」載入整個資料夾**。例如 review React 程式碼，不應載入 `vue-rules.*`。
2. **先 L1 後 L2**：先讀 compact，若資訊不足再讀詳細檔。
3. **引用 `_shared/`**：通用內容只讀一次，不重複展開。
4. **Wizard 結果寫入 `.agent/profile.json`** 後，後續任務讀此檔即可，不再觸發 wizard。
