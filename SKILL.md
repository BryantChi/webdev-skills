---
id: webdev_skills
name: webdev_skills
description: 完整的網站開發指南，涵蓋 UI/UX 設計、React/Vue/Next.js 前端框架、Laravel/Node.js 後端、Coding Style 與互動式選擇精靈
---

# 🌐 網站開發技能系統

> 適用於任何 AI 工具的完整網站開發指南

**📂 路徑說明：** 本文件中所有路徑皆相對於此 SKILL.md 所在目錄。讀取子檔案時，請以此目錄為基準解析路徑。每個 Phase 僅讀取當前需要的檔案（最多 3 個），避免一次載入過多內容。

**🎯 載入路徑表（Token 優化）：** 任務 → 最小檔案集對照請見 [`_shared/load-paths.md`](_shared/load-paths.md)。**不要為了「以防萬一」全載資料夾**。優先級制度與通用反模式分別見 [`_shared/priority-legend.md`](_shared/priority-legend.md) 與 [`_shared/anti-patterns.md`](_shared/anti-patterns.md)。

---

## 🚀 核心指令

**啟動指令：**

```text
幫我建立一個網站
```

**AI 行為：**
收到此指令後，將自動啟動下方的 **[標準作業程序 (SOP)](#-標準作業程序-sop)**。

➡️ [詳細工作流程參考](workflows/build-website.md)

---

## ⚠️ 核心設計原則

**所有設計必須遵守 [去 AI 感設計指南](ui/no-ai-feel.md)**：

- ❌ 不要過於完美對稱的佈局
- ❌ 不要庫存照片（握手、微笑商務人士）
- ❌ 不要空洞的商業文案
- ❌ 不要千篇一律的模板感
- ✅ 要有個性、有溫度、有故事感
- ✅ 要適度的不對稱和變化
- ✅ 要真實的內容和照片

---

## 🔄 標準作業程序 (SOP)

**觸發條件：** 僅在用戶明確要求「建立網站」、「Build Website」、「新專案」或類似**建立全新網站**的意圖時，才執行完整 SOP Phase 1-4。對於局部修改（如修改元件、調整樣式、修 Bug 等），直接應用相關設計原則即可，不需執行完整流程。

**互動協議 (Protocol)：**
當觸發完整 SOP 時，建議遵守以下執行順序。

### Phase 1: 智慧推理 (Reasoning)
> 💡 **建議**: 在寫任何 Code 之前，先完成此步驟以確保設計決策正確。

1. **分析意圖**：判斷專案類型 (SaaS/E-commerce/Portfolio/etc.)。
2. **執行推理**：讀取並應用 `logic/reasoning.md` 的規則。
3. **輸出決策**：展示 **Design Reasoning Block** (ASCII 表格)，確認風格、配色與反模式。

### Phase 2: 設計系統生成 (Design System)
> 🛑 **Constraint**: 必須建立單一真理來源 (Source of Truth)。

1. **讀取邏輯**：參照 `logic/design-system-gen.md`。
2. **生成檔案**：建立 `styles/design-system/MASTER.md`。
3. **定義變數**：確保色彩、字體、間距 tokens 已明確定義。

### Phase 3: 專案初始化 (Initialization)

1. **技術決策**：根據 Phase 1 推理結果，選擇最佳技術棧 (若用戶無指定)。
    - *SEO 優先* → Next.js
    - *快速開發* → Vue/Vite
    - *全端應用* → Laravel/Node
2. **建立專案**：執行初始化指令 (e.g., `npx create-next-app`).
3. **配置環境**：設定 Tailwind (`tailwind.config.js`) 連結至 `MASTER.md` 的變數。

### Phase 4: 開發執行 (Execution)

1. **元件開發**：優先製作基礎元件 (Button, Card, Input)。
2. **頁面組裝**：運用 `tpl/page/SKILL.md` 的結構進行開發。
3. **去 AI 感檢查**：隨時與 `ui/no-ai-feel.md` 對照，確保設計「有人味」。

---

## 🔧 手動/進階選項 (Manual Mode)

如果你希望能逐一步驟微調，或需要更細粒度的控制，可以使用互動精靈：

### 互動選擇精靈 ✨

| 精靈 | 說明 | 指令 |
|-----|------|-----|
| 🔧 [技術棧精靈](wizard/tech.md) | 選擇前後端框架 | `幫我選擇技術棧` |
| 🎨 [風格精靈](wizard/style.md) | 選擇設計風格 | `幫我選擇設計風格` |
| 📝 [Coding Style 精靈](wizard/code.md) | 選擇程式碼規範 | `幫我選擇 coding style` |

### 直接指定

告訴我你的需求，例如：
- `用 React + Tailwind 建立一個電商網站`
- `用 Laravel 建立 REST API`


---

## 📚 技能索引

### 核心邏輯 (New!)
| 類別 | 內容 |
|-----|------|
| [推理引擎](logic/reasoning.md) | 設計決策、反模式過濾 |
| [設計系統生成](logic/design-system-gen.md) | Master/Page 覆寫機制 |

### 設計相關
| 類別 | 內容 |
|-----|------|
| [UI 設計](ui/SKILL.md) | 色彩、排版、間距、55+ 設計風格 |
| [響應式設計](rwd/SKILL.md) | 斷點、行動優先、多裝置適配 |

### 前端開發
| 類別 | 內容 |
|-----|------|
| [前端框架](fe/SKILL.md) | React、Vue、Next.js、Nuxt.js、Vanilla |
| [CSS 架構](css/SKILL.md) | Tailwind、Bootstrap、SCSS、Vanilla |

### 後端開發
| 類別 | 內容 |
|-----|------|
| [後端框架](be/SKILL.md) | Laravel、Node.js、Django、FastAPI |
| [API 設計](api/SKILL.md) | REST、GraphQL、版本控制 |
| [資料庫](db/SKILL.md) | Schema 設計、ORM、遷移 |
| [安全性](sec/SKILL.md) | 驗證、授權、安全檢查清單 |

### 品質工程（New 2026-04）
| 類別 | 內容 |
|-----|------|
| [效能 / Core Web Vitals](perf/SKILL.md) | LCP/INP/CLS、Lighthouse、Bundle |
| [測試體系](test/SKILL.md) | Playwright、Vitest、視覺回歸、axe-core |
| [技術 SEO](seo/SKILL.md) | Meta/OG、JSON-LD、sitemap、canonical |
| [元件組合模式](comp/SKILL.md) | Compound、asChild、Headless |
| [多語系 i18n](i18n/SKILL.md) | next-intl、vue-i18n、react-intl |
| [部署 / DevOps](deploy/SKILL.md) | Vercel、Netlify、Docker、GitHub Actions |
| [可觀測性](obs/SKILL.md) | Sentry、Web Vitals RUM、結構化 logging |

### 應用功能（New 2026-04）
| 類別 | 內容 |
|-----|------|
| [狀態管理](state/SKILL.md) | Zustand、TanStack Query、Redux Toolkit、Pinia |
| [表單體系](form/SKILL.md) | RHF + Zod、TanStack Form、server actions、檔案上傳 |
| [認證授權](auth/SKILL.md) | Auth.js、Clerk、JWT/Cookie、RBAC/ABAC |
| [動畫](anim/SKILL.md) | Framer Motion、GSAP、View Transitions |

### 工作流程
| 類別 | 內容 |
|-----|------|
| [開發流程](flow/SKILL.md) | Git、測試、部署、效能優化 |
| [檢查清單](check/launch.md) | 上線、[Lighthouse](check/lighthouse.md)、[a11y](check/a11y.md)、[SEO](check/seo.md)、安全審計 |
| [Coding Style](code/SKILL.md) | JS、TS、PHP、Python、CSS 規範 |

### 模板
| 類別 | 內容 |
|-----|------|
| [元件模板](tpl/comp/SKILL.md) | 按鈕、表單、卡片、導航、Modal |
| [頁面模板](tpl/page/SKILL.md) | Landing、Dashboard、認證頁 |

---

## 📋 需求規格書

開始新專案前，建議先建立需求規格書：

➡️ [需求規格書模板](spec.md)

---

## 🤖 AI 工具使用指南

不同的 AI 編程助手載入方式不同：

| AI 工具 | 載入方式 |
|--------|----------|
| **Cursor** | `@.agent/skills/webdev/SKILL.md` |
| **Windsurf** | `@.agent/skills/webdev/SKILL.md` |
| **Copilot** | `#file:.agent/skills/webdev/SKILL.md` |
| **Claude** | Projects 上傳或貼上內容 |
| **ChatGPT** | 建立 GPT 或貼上內容 |

➡️ [完整 AI 工具使用指南](ai-tools-guide.md)

---

## 🌐 生態整合（可組合的官方/業界 Skills）

本 Skill 庫為「整合包」，若要加深特定能力，可額外安裝下列業界可信 Skills：

| Skill | 來源 | 補強能力 |
|-------|------|---------|
| `frontend-design` | Anthropic 官方 | 設計理念與大膽美學承諾 |
| `webapp-testing` / `playwright` | Anthropic 官方 | E2E 自動化（與本庫 [test/](test/SKILL.md) 互補） |
| `vercel-labs/react-best-practices` | Vercel | 57 條效能規則（與 [fe/react-rules.critical.md](fe/react-rules.critical.md) 互補） |
| `vercel-labs/web-design-guidelines` | Vercel | 100+ a11y/UX 稽核 |
| `addyosmani/web-quality-skills` | Addy Osmani | Lighthouse + CWV 自動化 |
| `bitjaru/styleseed` | 社群 | shadcn 品牌 token 預設（Toss/Stripe/Linear/...） |

> 安裝方式參考各官方 repo。本庫已透過載入路徑表確保不會與這些 Skills 重複觸發。

---

## 版本資訊

- 版本：1.5.0
- 更新日期：2026-04-26
- 語言：繁體中文
- 設計風格：55+ 種
- 模組總計：26 個子目錄（含品質工程 7、應用功能 4、_shared 1）
- 品質工程模組：perf、test、seo、comp、i18n、deploy、obs
- 應用功能模組：state、form、auth、anim
- 共享資源：_shared（priority-legend、anti-patterns、verification-commands、load-paths）
- 規則庫：fe/{react,vue,next}-rules.{critical,optional}.md
- Workflow：`workflows/build-website.md` 升級為 15 階段（含測試、效能、SEO、部署、監控）
- Wizard：`wizard/tech.md` 升級為 15 題（新增測試、部署、效能目標 3 題）
- Token 優化：三層級載入（L0 index / L1 compact / L2 full），單次任務消耗目標下降 25-60%

