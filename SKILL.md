---
name: 網站開發技能系統
description: 完整的網站開發指南，涵蓋 UI/UX、前後端框架、Coding Style，支援互動式選擇精靈
---

# 🌐 網站開發技能系統

> 適用於任何 AI 工具的完整網站開發指南

---

## 🚀 一鍵啟動完整流程

**說這句話即可啟動完整網站開發流程：**

```text
幫我建立一個網站
```

或使用 workflow 指令：`/build-website`

→ 將自動執行：需求收集 → 技術選擇 → 風格確定 → 開發 → 檢查

➡️ [完整工作流程說明](../../workflows/build-website.md)

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

## 快速開始

### 方式一：精簡模式（省 Token）⚡

載入單一檔案即可開始開發：

```text
讀取 .agent/skills/webdev/compact.md
```

包含：核心原則 + 風格速查 + 設計 Token + 元件模板（~2000 tokens）

另有 [風格色碼速查](ui/styles/colors.md) 供快速查詢配色

### 方式二：完整流程（推薦）

說「幫我建立網站」啟動 12 步完整流程

### 方式二：互動選擇精靈 ✨

不確定要用什麼技術？讓精靈引導你：

| 精靈 | 說明 | 指令 |
|-----|------|-----|
| 🔧 [技術棧精靈](wizard/tech.md) | 選擇前後端框架 | `幫我選擇技術棧` |
| 🎨 [風格精靈](wizard/style.md) | 選擇設計風格 | `幫我選擇設計風格` |
| 📝 [Coding Style 精靈](wizard/code.md) | 選擇程式碼規範 | `幫我選擇 coding style` |

### 方式三：直接指定

告訴我你的需求，例如：
- `用 React + Tailwind 建立一個電商網站`
- `用 Laravel 建立 REST API`
- `用 Vue + 玻璃擬態風格建立作品集`

### 方式四：根據描述判斷

描述你的專案，我會根據以下因素推薦最適合的方案：
- 專案類型與規模
- SEO 與效能需求
- 團隊技術背景
- 開發時程與預算

---

## 📚 技能索引

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

### 工作流程
| 類別 | 內容 |
|-----|------|
| [開發流程](flow/SKILL.md) | Git、測試、部署、效能優化 |
| [檢查清單](check/launch.md) | 上線前、SEO、安全審計 |
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

## 版本資訊

- 版本：1.1.0
- 更新日期：2026-01-22
- 語言：繁體中文
- 總檔案數：102
- 設計風格：55 種

