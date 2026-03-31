# 🌐 Webdev Skills System

> 網站開發 AI 技能系統 - 適用於 Claude Code、Codex、Gemini、OpenCode、Cursor、Windsurf 等 AI 工具

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![繁體中文](https://img.shields.io/badge/語言-繁體中文-blue.svg)]()

## ✨ 特色

- 🎨 **55 種設計風格** - 從極簡到賽博龐克，應有盡有
- 🧙 **互動式精靈** - 3 套問答引導選擇技術棧、風格、規範
- 🚫 **去 AI 感設計** - 確保設計有人味，不像機器生成
- ⚡ **Token 優化** - 精簡模式只需 ~2000 tokens
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
├── compact.md            # ⚡ 精簡版 (~2000 tokens)
├── spec.md               # 📋 需求規格書模板
│
├── logic/                # 🧠 核心邏輯
│   ├── reasoning.md      # 設計推理引擎
│   └── design-system-gen.md  # 設計系統生成器
│
├── wizard/               # 🧙 互動精靈
│   ├── tech.md           # 技術棧選擇 (12 題)
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
├── fe/                   # 🖥️ 前端 (React, Vue, Next.js, Nuxt.js, Vanilla)
├── be/                   # 🔧 後端 (Laravel, Node.js, Django, FastAPI)
├── css/                  # 🎨 CSS (Tailwind, Bootstrap, SCSS, Vanilla)
├── api/                  # 🔌 API 設計
├── db/                   # 🗄️ 資料庫
├── sec/                  # 🔒 安全性
├── rwd/                  # 📱 響應式設計
├── flow/                 # 🔄 開發流程
├── code/                 # 📝 Coding Style
├── check/                # ✅ 檢查清單
├── tpl/                  # 📄 元件/頁面模板
└── workflows/            # 🚀 工作流程
    └── build-website.md  # 12 步完整開發流程
```

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

## ⚡ Token 優化

| 模式 | Tokens | 使用場景 |
|-----|--------|---------|
| 精簡模式 | ~2,000 | ChatGPT Free、快速開發 |
| 單一風格 | ~3,000 | 已知風格 |
| 完整流程 | ~8,000 | Claude Pro、Cursor |

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

### 完整開發流程

```text
幫我建立一個網站
```

AI 會自動執行 SOP：需求收集 → 設計推理 → 技術選擇 → 設計系統 → 開發

### 快速指定

```text
用 Next.js + Tailwind + 玻璃擬態風格建立作品集
```

### 使用精靈

```text
幫我選擇技術棧
幫我選擇設計風格
幫我選擇 coding style
```

## ⚠️ 核心原則：去 AI 感

所有設計必須遵守 [no-ai-feel.md](ui/no-ai-feel.md)：

- ❌ 不要完美對稱佈局
- ❌ 不要庫存照片
- ❌ 不要空洞商業文案
- ✅ 要有個性和記憶點
- ✅ 要適度的不對稱
- ✅ 要真實的內容

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

## 📄 License

MIT License - 自由使用、修改、分發

---

Made with ❤️ for better web development
