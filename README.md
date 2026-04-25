# 🌐 Webdev Skills System

> 網站開發 AI 技能系統 - 適用於 Claude Code、Codex、Gemini、OpenCode、Cursor、Windsurf 等 AI 工具

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![繁體中文](https://img.shields.io/badge/語言-繁體中文-blue.svg)]()

## ✨ 特色（v1.5）

- 🎨 **55+ 種設計風格** - 從極簡到賽博龐克，應有盡有
- 🧙 **互動式精靈** - 3 套問答（15+15+15 題）引導選擇技術棧、風格、規範
- 🚫 **去 AI 感設計** - 三段式禁用 / 替代 / 反例對比，對齊 frontend-design
- ⚡ **三層級 Token 優化** - L0/L1/L2 漸進載入 + `.agent/profile.json` 快取，單次任務消耗下降 25-60%
- 📐 **Priority-tagged 規則庫** - React/Vue/Next 必修規則拆 critical/optional 兩檔
- 🔬 **品質工程模組（7）** - 效能（CWV）、測試（Playwright/axe）、SEO（JSON-LD）、元件組合、i18n、部署、可觀測性
- 🎯 **應用功能模組（4）** - 狀態管理（Zustand/TanStack Query）、表單（RHF+Zod）、認證（Auth.js/Clerk）、動畫（Framer Motion/GSAP）
- 🚀 **完整建站 SOP** - 15 階段流程，從推理 → 設計系統 → 開發 → 測試 → 效能 → SEO → 部署 → 監控
- 🌐 **業界生態整合** - 與 Anthropic frontend-design、Vercel React Best Practices、addyosmani Web Quality 互補
- 🔧 **全工具支援** - CLI (Claude Code / Codex / Gemini / OpenCode) + IDE (Cursor / Windsurf / Antigravity)

## 📦 安裝

### 方式一：全域安裝（CLI 工具推薦）

將 skill 安裝到 AI 工具的全域 skills 目錄，所有專案皆可使用：

```bash
# Claude Code CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.claude/skills/webdev_skills

# Codex CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.codex/skills/webdev_skills

# Gemini CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.gemini/skills/webdev_skills

# OpenCode CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.opencode/skills/webdev_skills
```

### 方式二：專案級安裝（IDE 工具推薦）

```bash
# 在你的專案根目錄
mkdir -p .agent/skills
git submodule add https://github.com/BryantChi/webdev-skills.git .agent/skills/webdev

# 或直接 clone
git clone https://github.com/BryantChi/webdev-skills.git .agent/skills/webdev
```

### 方式三：手動下載

下載 ZIP 並解壓到對應目錄。

## 🚀 快速開始

### CLI 工具

全域安裝後，AI 會自動識別 skill。直接對話即可：

```text
幫我建立一個網站
```

### Cursor / Windsurf

```text
@.agent/skills/webdev/SKILL.md 幫我建立一個網站
```

### VS Code + Copilot

```text
#file:.agent/skills/webdev/SKILL.md 幫我建立網站
```

### Claude.ai / ChatGPT

將 `compact.md` 內容貼到對話中，或上傳到 Projects/GPTs。

## 📁 目錄結構

```
webdev_skills/
├── SKILL.md              # 📍 主入口 (SOP)
├── compact.md            # ⚡ L1 精簡版 (~2500 tokens)，含新模組速查
├── spec.md               # 📋 需求規格書模板
│
├── logic/                # 🧠 核心邏輯
│   ├── reasoning.md      # 設計推理引擎
│   └── design-system-gen.md  # 設計系統生成器
│
├── wizard/               # 🧙 互動精靈
│   ├── tech.md           # 技術棧選擇 (15 題，含測試/部署/效能目標)
│   ├── style.md          # 設計風格選擇 (15 題)
│   └── code.md           # Coding Style 選擇 (15 題)
│
├── ui/                   # 🎨 UI 設計
│   ├── color.md          # 色彩系統
│   ├── typo.md           # 字型排版
│   ├── space.md          # 間距系統
│   ├── a11y.md           # 無障礙設計
│   ├── no-ai-feel.md     # ⚠️ 去 AI 感指南
│   └── styles/           # 55 種設計風格
│
├── _shared/              # 🔁 跨模組共享（priority、anti-patterns、verification、load-paths）
├── fe/                   # 🖥️ 前端 (React, Vue, Next.js, Nuxt.js, Vanilla) + *-rules.{critical,optional}.md
├── be/                   # 🔧 後端 (Laravel, Node.js, Django, FastAPI)
├── css/                  # 🎨 CSS (Tailwind, Bootstrap, SCSS, Vanilla, shadcn-radix)
├── comp/                 # 🧩 元件組合模式 (Compound, Slot/asChild, Headless) — New
├── api/                  # 🔌 API 設計
├── db/                   # 🗄️ 資料庫
├── sec/                  # 🔒 安全性
├── rwd/                  # 📱 響應式設計
├── perf/                 # ⚡ Core Web Vitals + Lighthouse + Bundle — New
├── test/                 # 🧪 Playwright + Vitest + 視覺回歸 + axe-core — New
├── seo/                  # 🔍 Meta/OG + JSON-LD + Sitemap + Canonical — New
├── i18n/                 # 🌐 next-intl + vue-i18n + react-intl — New
├── deploy/               # 🚀 Vercel + Netlify + Docker + GitHub Actions — New
├── obs/                  # 📡 Sentry + Web Vitals RUM + Logging — New
├── state/                # 🗂️ Zustand + TanStack Query + RTK + Pinia — New
├── form/                 # 📝 RHF + Zod + TanStack Form + 檔案上傳 — New
├── auth/                 # 🔐 Auth.js + Clerk + JWT/Cookie + RBAC — New
├── anim/                 # 🎬 Framer Motion + GSAP + View Transitions — New
├── flow/                 # 🔄 開發流程
├── code/                 # 📝 Coding Style
├── check/                # ✅ 檢查清單（launch、lighthouse、a11y、seo）
├── tpl/                  # 📄 元件/頁面模板
└── workflows/            # 🚀 工作流程
    └── build-website.md  # 15 階段完整開發流程
```

### 🔁 `_shared/` 跨模組共享資源

所有子模組都會引用這四份共享檔，避免重複內容、確保規範一致：

| 檔案 | 用途 |
|------|------|
| [`_shared/priority-legend.md`](_shared/priority-legend.md) | 規則優先級定義（CRITICAL / HIGH / MEDIUM / LOW） |
| [`_shared/anti-patterns.md`](_shared/anti-patterns.md) | 通用反模式速查（跨框架、跨領域） |
| [`_shared/verification-commands.md`](_shared/verification-commands.md) | 驗證指令集（lint / test / build / audit） |
| [`_shared/load-paths.md`](_shared/load-paths.md) | 任務 → 最小檔案集對照表（避免一次全載） |

> **載入策略**：任何規則類檔案先看 `priority-legend.md` 理解優先級，再依任務類型查 `load-paths.md` 決定要讀哪幾個 `compact.md`。

---

## 🌐 業界對照表（與本庫定位）

| 業界知名 Skill | 來源 | 對應本庫模組 |
|---------------|------|--------------|
| `frontend-design` (277K+ installs) | Anthropic 官方 | [`ui/no-ai-feel.md`](ui/no-ai-feel.md) + [`comp/`](comp/SKILL.md) |
| `react-best-practices` (57 條規則) | Vercel | [`fe/react-rules.critical.md`](fe/react-rules.critical.md) + [`fe/react-rules.optional.md`](fe/react-rules.optional.md) |
| `web-design-guidelines` (100+ a11y/UX) | Vercel | [`check/a11y.md`](check/a11y.md) + [`test/a11y-axe.md`](test/a11y-axe.md) |
| `composition-patterns` | Vercel | [`comp/{compound,slot,headless}.md`](comp/SKILL.md) |
| `web-quality-skills` (Lighthouse/CWV) | addyosmani | [`perf/`](perf/SKILL.md) + [`obs/web-vitals-reporter.md`](obs/web-vitals-reporter.md) |
| `webapp-testing` / `playwright` | Anthropic 官方 | [`test/e2e-playwright.md`](test/e2e-playwright.md)（含 Playwright MCP 整合） |
| `styleseed` (69 設計規則 + 5 品牌) | bitjaru | [`css/shadcn-radix.md`](css/shadcn-radix.md) + 55+ 風格庫 |
| `shadcn/ui Skills` (CLI v4) | shadcn 官方 | [`css/shadcn-radix.md`](css/shadcn-radix.md) |
| `Trail of Bits Security` | ToB | [`sec/SKILL.md`](sec/SKILL.md) + [`_shared/anti-patterns.md`](_shared/anti-patterns.md) |
| `superpowers` (TDD/verify) | Anthropic | 與本庫流程相容（建議搭配） |

> **本庫定位**：自製整合包（一次涵蓋設計→開發→測試→效能→SEO→部署→監控全流程），業界 Skills 為單點專精。可組合使用。

## 🎨 55 種設計風格

<details>
<summary>點擊展開完整列表</summary>

### 基礎風格 (01-10)
極簡、扁平化、Material、玻璃擬態、新擬物、野獸派、漸層、暗黑、亮白、雙色調

### 產業風格 (11-25)
企業、新創、電商、SaaS、儀表板、作品集、部落格、媒體、醫療、教育、金融、法律、餐飲、旅遊、健身

### 藝術風格 (26-35)
復古、古典、裝飾藝術、孟菲斯、波普、有機、水彩、線條、插畫、3D

### 文化風格 (36-42)
日式禪風、北歐、瑞士、中式傳統、中式現代、波希米亞、熱帶

### 科技風格 (43-50)
賽博龐克、科幻、科技極簡、AI、區塊鏈、遊戲、VR/AR、霓虹

### 特殊風格 (51-55)
環保、奢華、童趣、編輯、無邊框

</details>

## ⚡ Token 優化（v1.5 三層級架構）

| 場景 | 載入檔案 | Tokens | 適用 |
|------|---------|--------|------|
| **L0 啟動** | `SKILL.md`（路由表） | ~800 | 預設 — AI 依任務再讀子模組 |
| **L1 精簡** | `SKILL.md` + `compact.md` + 任務對應 `<module>/compact.md` | ~1,500-2,500 | Token 受限、ChatGPT Free |
| **L1 + 1 任務** | + 1-2 個 L2 詳細檔 | ~2,000-5,000 | 一般任務（改色、加效能、寫測試） |
| **L2 完整流程** | 完整建站 SOP 15 階段 | ~9,000 | Claude Pro、Cursor、Windsurf |
| **全量** | 整個目錄 | ~25,000 | 大上下文 Agent（Roo / Goose） |

**任務 → 最小檔案集** 對照見 [`_shared/load-paths.md`](_shared/load-paths.md)。**不要為了「以防萬一」全載目錄**。

> **關鍵改進**：v1.5 採漸進式揭露（Progressive Disclosure），相同任務 token 消耗較 v1.0 下降 25-60%，但能力上限擴大 3 倍（24 個專業模組）。

## 🤖 支援的 AI 工具

| 工具 | 類型 | 相容性 | 安裝位置 |
|-----|------|--------|---------|
| Claude Code | CLI | ✅ | `~/.claude/skills/webdev_skills/` |
| Codex CLI | CLI | ✅ | `~/.codex/skills/webdev_skills/` |
| Gemini CLI | CLI | ✅ | `~/.gemini/skills/webdev_skills/` |
| OpenCode CLI | CLI | ✅ | `~/.opencode/skills/webdev_skills/` |
| Antigravity | IDE | ✅ | `.agent/skills/webdev/` (專案內) |
| Cursor | IDE | ✅ | `.agent/skills/webdev/` (專案內) |
| Windsurf | IDE | ✅ | `.agent/skills/webdev/` (專案內) |
| VS Code + Copilot | IDE | ✅ | `#file:` 引用 |
| Aider | CLI | ✅ | `--read` 載入 |
| Claude.ai / ChatGPT | Web | ✅ | 貼上或上傳 |

詳見 [ai-tools-guide.md](ai-tools-guide.md)

## 📖 使用指南

### 完整開發流程（15 階段）

```text
幫我建立一個網站
```

AI 自動執行 SOP：需求收集 → 設計推理 → 設計系統 → 專案初始化 → 元件 → 頁面 → 後端 → RWD/a11y → **測試** → **效能** → **SEO** → 上線檢查 → **部署+監控**（粗體為 v1.5 新增階段）

### 快速指定

```text
用 Next.js + Tailwind + shadcn/ui + 玻璃擬態風格建立作品集
```

### 使用精靈（結果會寫入 `.agent/profile.json`，下次不重問）

```text
幫我選擇技術棧
幫我選擇設計風格
幫我選擇 coding style
```

### 單點任務（v1.5 新增）

```text
幫我做效能優化（LCP/INP/CLS）
幫我寫 Playwright E2E 測試
幫我加 SEO（meta + JSON-LD + sitemap）
幫我設定 Sentry 錯誤監控
幫我加 i18n 多語系
幫我設定 CI/CD（GitHub Actions）
幫我用 React Hook Form + Zod 寫表單
幫我接 Auth.js 登入系統
幫我用 shadcn/ui 重構元件
幫我加路由轉場動畫
```

每個指令會自動載入對應模組的最小檔案集（見 [`_shared/load-paths.md`](_shared/load-paths.md)）。

## ⚠️ 核心原則：去 AI 感

所有設計必須遵守 [no-ai-feel.md](ui/no-ai-feel.md) 的**三段式速查**（v1.5 對齊 Anthropic frontend-design）：

- **Section A 禁用清單**：Inter/Roboto 等 AI 字體、紫藍漸層白底、完美對稱 3 欄、「專業領先最佳」空話、庫存照片
- **Section B 替代方案**：Cabinet Grotesk 等個性字體、大膽品牌色 + 反差強調色、不對稱 5fr/7fr、真實照片
- **Section C 反例對比**：每條禁用都附「❌ AI 感 → ✅ 有人味」一句決勝對照

並對齊「Intent → Bold Direction → Refinement」工作流，而非平庸中庸。

## 🔄 更新

```bash
# 全域安裝更新
cd ~/.claude/skills/webdev_skills && git pull

# 專案 Submodule 更新
git submodule update --remote .agent/skills/webdev
```

## 🔧 自訂

### 命名規範

所有 `.md` 檔案的 YAML frontmatter 必須包含以下欄位：

```yaml
---
id: glassmorphism              # snake_case 英文，CLI 工具用此匹配
name: Glassmorphism            # English Title Case，顯示名稱
description: 玻璃擬態設計風格   # 中文 + 英文技術詞
---
```

| 欄位 | 格式 | 用途 |
|------|------|------|
| `id` | `snake_case` 英文 | CLI 工具識別碼（如 `/webdev_skills`） |
| `name` | English Title Case | 顯示名稱（不可含中文） |
| `description` | 中文 + 英文技術詞 | AI 自動判斷何時載入此 skill |

> **重要**：`name` 不可使用中文或包含空格以外的特殊字元，否則 CLI 工具會無法辨識。

### 新增內容

- **新增風格**：在 `ui/styles/` 新增 `XX-name.md`
- **新增框架**：在 `fe/` 或 `be/` 新增對應 `.md`
- **修改精靈**：編輯 `wizard/` 中的問題
- **新增規則類檔案**：拆 `*.critical.md` 與 `*.optional.md` 兩檔（critical = CRITICAL+HIGH 預設載入；optional = MEDIUM+LOW 僅 review/refactor 載入）
- **新增模組（如 `state/`）**：必含三層級檔案（`SKILL.md` 路由 + `compact.md` 精煉 + 詳細檔），引用 `_shared/{priority-legend,anti-patterns,verification-commands}.md`
- **規則優先級制度**：CRITICAL（必修）/ HIGH（強建議）/ MEDIUM / LOW，定義見 [`_shared/priority-legend.md`](_shared/priority-legend.md)

### 安裝後第一步

| 用途 | 起點 |
|------|------|
| 完整建站 | 對 AI 說「幫我建立網站」→ 走 [`workflows/build-website.md`](workflows/build-website.md) 15 階段 |
| 加效能 / 測試 / SEO | [`_shared/load-paths.md`](_shared/load-paths.md) 對應任務的最小檔案集 |
| AI 工具設定 | [`ai-tools-guide.md`](ai-tools-guide.md) |
| 規格書模板 | [`spec.md`](spec.md) |

## 📄 License

MIT License - 自由使用、修改、分發

## 📊 版本資訊

- **版本**：1.5.0（2026-04-26）
- **模組**：27 個目錄、183 個 `.md` 檔
- **連結完整性**：585 個內部連結，**0 破損**
- **歷史**：
  - v1.5 — 新增 4 個應用功能模組（state/form/auth/anim）
  - v1.4 — 新增 7 個品質工程模組（perf/test/seo/comp/i18n/deploy/obs）+ `_shared/` 共享 + Priority-tagged rules
  - v1.3 — Workflow 升級為 15 階段、Wizard 升級為 15 題
  - v1.2 — 三層級 Token 優化架構

---

Made with ❤️ for better web development
