# 🌐 Webdev Skills System

> 網站開發 AI 技能系統 - 適用於 Cursor、Windsurf、Claude、ChatGPT 等 AI 編程助手

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![繁體中文](https://img.shields.io/badge/語言-繁體中文-blue.svg)]()

## ✨ 特色

- 🎨 **55 種設計風格** - 從極簡到賽博龐克，應有盡有
- 🧙 **互動式精靈** - 3 套問答引導選擇技術棧、風格、規範
- 🚫 **去 AI 感設計** - 確保設計有人味，不像機器生成
- ⚡ **Token 優化** - 精簡模式只需 ~2000 tokens
- 🔧 **多工具支援** - Cursor、Windsurf、VS Code、Claude、ChatGPT 等

## 🗣️ 如何觸發此技能？

**支援的觸發詞 (Aliases)：**

你可以在任何支援 Skills 的 AI 工具中，使用以下詞彙來觸發此技能系統：

| 語言 | 關鍵字 |
|------|--------|
| English | `webdev`, `web-development`, `frontend`, `backend` |
| 中文 | `網站開發`, `網頁設計` |

**範例：**

```text
使用 webdev skill 幫我建立網站
```

```text
啟動網站開發技能
```

## 📦 安裝

### 方式一：Clone 到專案

```bash
# 在你的專案根目錄
git clone https://github.com/YOUR_USERNAME/webdev-skills.git .agent/skills/webdev

# 或使用 submodule
git submodule add https://github.com/YOUR_USERNAME/webdev-skills.git .agent/skills/webdev
```

### 方式二：直接下載

下載 ZIP 並解壓到 `.agent/skills/webdev/` 目錄

### 方式三：複製整個目錄

```bash
cp -r webdev-skills/ YOUR_PROJECT/.agent/skills/webdev/
```

## 🚀 快速開始

### Cursor / Windsurf

```text
@.agent/skills/webdev/SKILL.md 幫我建立一個網站
```

### VS Code + Copilot

```text
#file:.agent/skills/webdev/SKILL.md 幫我建立網站
```

### Claude / ChatGPT

將 `compact.md` 內容貼到對話中，或上傳到 Projects/GPTs

### 精簡模式 (省 Token)

```text
讀取 .agent/skills/webdev/compact.md
```

## 📁 目錄結構

```
.agent/skills/webdev/
├── workflows/
│   └── build-website.md  # 12 步完整開發流程
│
    ├── SKILL.md              # 📍 主入口
    ├── compact.md            # ⚡ 精簡版 (~2000 tokens)
    ├── ai-tools-guide.md     # 🤖 AI 工具使用指南
    ├── spec.md               # 📋 需求規格書模板
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
    │       ├── index.md      # 風格索引
    │       ├── colors.md     # 風格色碼速查
    │       └── 01-55-*.md    # 各風格詳細指南
    │
    ├── fe/                   # 🖥️ 前端框架
    │   ├── react.md, vue.md, next.md, nuxt.md, vanilla.md
    │
    ├── css/                  # 🎨 CSS 架構
    │   ├── tailwind.md, bootstrap.md, scss.md, vanilla.md
    │
    ├── be/                   # 🔧 後端框架
    │   ├── laravel.md, node.md, django.md, fastapi.md
    │
    ├── api/                  # 🔌 API 設計
    ├── db/                   # 🗄️ 資料庫
    ├── sec/                  # 🔒 安全性
    ├── rwd/                  # 📱 響應式設計
    ├── flow/                 # 🔄 開發流程
    ├── check/                # ✅ 檢查清單
    ├── code/                 # 📝 Coding Style
    └── tpl/                  # 📄 模板
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

| 工具 | 相容性 | 載入方式 |
|-----|--------|---------|
| Cursor | ✅ | @file 引用 |
| Windsurf | ✅ | @file 引用 |
| VS Code + Copilot | ✅ | #file 引用 |
| Claude | ✅ | Projects/貼上 |
| ChatGPT | ⚠️ | GPTs/貼上 |
| Aider | ✅ | /add 指令 |
| Claude CLI | ✅ | 自動讀取 |

詳見 [ai-tools-guide.md](skills/webdev/ai-tools-guide.md)

## 📖 使用指南

### 完整開發流程

```text
幫我建立一個網站
```

AI 會自動執行 12 步流程：需求收集 → 技術選擇 → 風格確定 → 開發...

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

所有設計必須遵守 [no-ai-feel.md](skills/webdev/ui/no-ai-feel.md)：

- ❌ 不要完美對稱佈局
- ❌ 不要庫存照片
- ❌ 不要空洞商業文案
- ✅ 要有個性和記憶點
- ✅ 要適度的不對稱
- ✅ 要真實的內容

## 🔧 自訂

### 新增風格

在 `ui/styles/` 新增 `XX-name.md`，參考現有格式

### 新增框架

在 `fe/` 或 `be/` 新增對應的 `.md` 檔案

### 修改精靈

編輯 `wizard/` 中的問題

## 📄 License

MIT License - 自由使用、修改、分發

## 🙏 貢獻

歡迎 PR 和 Issue！

---

Made with ❤️ for better web development
